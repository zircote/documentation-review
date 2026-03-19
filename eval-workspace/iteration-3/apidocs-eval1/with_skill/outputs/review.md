# FastAPI OpenAPI Spec Review: Uncustomized Auto-Generated Documentation

**Review date:** 2026-03-19
**Target:** `src/main.py` -- FastAPI application with ~20 endpoints, default auto-generated OpenAPI output
**Methodology:** API Documentation skill review workflow + quality checklist

---

## Executive Summary

FastAPI auto-generates an OpenAPI 3.1 spec from your route decorators and Pydantic models. Out of the box, this gives you a structurally valid spec, but it leaves most of the documentation surface area at default values. The result is a spec that tooling can parse but humans cannot effectively use. Below is a systematic review of what is missing and concrete guidance for each gap.

---

## Review Workflow Results

### Step 1: Inventory Endpoints

FastAPI will include every `@app.get(...)`, `@app.post(...)`, etc. in the generated spec, so the path inventory is likely complete. However, verify:

- **Routers mounted with `include_router()`** -- endpoints from sub-routers appear under whatever prefix you set. Confirm all routers are mounted and that prefix paths are intentional.
- **Middleware-only endpoints** (health checks, metrics) -- these sometimes bypass the router and will be missing from the spec.
- **WebSocket endpoints** -- `@app.websocket(...)` routes do not appear in the OpenAPI spec at all. If you have any, they need separate documentation (AsyncAPI or manual).

**Action:** Run your app and hit `/openapi.json`. Count the paths. If fewer than 20, you have missing endpoints.

### Step 2: Check Required Elements (Per-Endpoint)

For each of the 7 required elements, here is what FastAPI auto-generates vs. what you need to add:

| Element | Auto-Generated Default | What Is Missing |
|---------|----------------------|-----------------|
| **HTTP Method + Path** | Yes, from decorators | Nothing -- this works out of the box |
| **Summary** | Function name converted to title case (e.g., `get_users` becomes "Get Users") | Meaningful one-line descriptions. "Get Users" is a label, not a summary. Write: "Retrieve a paginated list of users filtered by status and role." |
| **Description** | Empty unless you set the docstring | Every endpoint needs a description explaining behavior, side effects, and constraints. Use the function docstring or the `description` parameter. |
| **Parameters** | Path/query params extracted from function signature with types | Missing: `description` for each parameter, `example` values, `minimum`/`maximum` constraints, `default` clarification, `enum` values for constrained strings |
| **Request Body** | Schema generated from Pydantic model if used | Missing: `example` values on the schema or request body, `description` on individual fields in the Pydantic model |
| **Responses** | Only `200` with `"Successful Response"` and `422` with `"Validation Error"` | Missing: `400`, `401`, `403`, `404`, `409`, `500` responses. The auto-generated 422 is a FastAPI internal detail, not a substitute for documenting your actual error responses. |
| **Examples** | None | No request or response examples are generated. These must be added explicitly. |

**This is the largest gap.** A 20-endpoint app with defaults has roughly 120+ missing documentation elements (6 gaps per endpoint).

### Step 3: Validate Examples

Since no examples exist in the default output, this step is blocked. Once you add examples, validate that:

- All JSON examples parse without errors
- Examples match the declared schema (field names, types, required fields)
- Examples use realistic values, not placeholders like `"string"` or `0`

### Step 4: Audit Error Responses

**This is the most impactful deficiency.** FastAPI's auto-generated spec only declares:

- `200 Successful Response` (or `201` for POST if you set `status_code=201`)
- `422 Validation Error` (FastAPI's built-in request validation)

Missing from every endpoint:

| Response Code | When to Document | Impact of Omission |
|---------------|-----------------|---------------------|
| `400 Bad Request` | Business logic validation failures (distinct from 422 schema validation) | Consumers cannot distinguish schema errors from business rule violations |
| `401 Unauthorized` | Any authenticated endpoint | Consumers do not know auth is required until they get a runtime error |
| `403 Forbidden` | Endpoints with role/permission checks | Consumers cannot tell "not logged in" from "not allowed" |
| `404 Not Found` | Any endpoint with a path parameter (`/users/{id}`) | Consumers have no documented behavior for missing resources |
| `409 Conflict` | Create/update endpoints where uniqueness constraints apply | Consumers cannot handle duplicate-entry errors |
| `429 Too Many Requests` | If you have rate limiting middleware | Consumers cannot implement retry/backoff logic |
| `500 Internal Server Error` | All endpoints | Consumers have no fallback error schema |

**Recommendation:** Define a reusable `Error` schema in `components/schemas` and `$ref` it from all error responses:

```python
from pydantic import BaseModel
from typing import Optional

class ErrorDetail(BaseModel):
    field: Optional[str] = None
    message: str

class ErrorResponse(BaseModel):
    """Standardized error response body."""
    code: str  # Machine-readable: "NOT_FOUND", "VALIDATION_ERROR", etc.
    message: str  # Human-readable explanation
    details: Optional[list[ErrorDetail]] = None

    model_config = {
        "json_schema_extra": {
            "examples": [
                {
                    "code": "NOT_FOUND",
                    "message": "The requested user was not found",
                    "details": None
                }
            ]
        }
    }
```

Then reference it in your route decorators:

```python
@app.get(
    "/users/{user_id}",
    responses={
        404: {"model": ErrorResponse, "description": "User not found"},
        401: {"model": ErrorResponse, "description": "Authentication required"},
    },
)
```

### Step 5: Verify Authentication

FastAPI's auto-generated spec includes **no security schemes** unless you explicitly define them. If your app uses any authentication (JWT, API keys, OAuth2), none of that is visible in the spec.

**What to add:**

1. Define security schemes in the `FastAPI()` constructor or via `components`:

```python
from fastapi import FastAPI
from fastapi.openapi.utils import get_openapi

app = FastAPI()

# Option A: Use FastAPI's built-in OAuth2/APIKey dependencies
from fastapi.security import HTTPBearer, APIKeyHeader

bearer_scheme = HTTPBearer()
api_key_scheme = APIKeyHeader(name="X-API-Key")

# Option B: Customize the OpenAPI schema directly
def custom_openapi():
    if app.openapi_schema:
        return app.openapi_schema
    schema = get_openapi(
        title="Your API",
        version="1.0.0",
        routes=app.routes,
    )
    schema["components"]["securitySchemes"] = {
        "bearerAuth": {
            "type": "http",
            "scheme": "bearer",
            "bearerFormat": "JWT",
        }
    }
    schema["security"] = [{"bearerAuth": []}]
    app.openapi_schema = schema
    return schema

app.openapi = custom_openapi
```

2. For public endpoints, explicitly mark them with `security: []` override.

3. Document scopes if using OAuth2 with role-based access.

### Step 6: Quality Checklist (Final Pass)

#### Completeness

- [ ] **All endpoints documented** -- Likely complete due to auto-generation, but verify router mounts and WebSocket routes.
- [ ] **All parameters described** -- FAIL. Auto-generated params have types but no `description`, no `example`, no constraints beyond type.
- [ ] **All response codes listed** -- FAIL. Only 200 and 422 are present. At minimum, add 400, 401, 404, 500 to applicable endpoints.
- [ ] **Authentication explained** -- FAIL. No `securitySchemes` defined. No global or per-endpoint security applied.
- [ ] **Rate limits documented** -- FAIL. No rate limit headers or 429 response documented.
- [ ] **Pagination pattern documented** -- FAIL. List endpoints likely return arrays but do not document pagination parameters (`page`, `limit`, `offset`) or the pagination response envelope.

#### Accuracy

- [ ] **Schemas match actual responses** -- PARTIAL. Pydantic models generate accurate schemas, but only for the success case. Error response schemas are not defined.
- [ ] **Examples are valid JSON** -- FAIL. No examples exist.
- [ ] **Status codes are correct** -- PARTIAL. The 200 responses are correct, but POST endpoints returning created resources should use 201, and DELETE should use 204. Check that `status_code` is set on each decorator.
- [ ] **Parameter types are accurate** -- PASS (if Pydantic models and type hints are correct). FastAPI derives these from Python types.

#### Usability

- [ ] **Clear summaries for endpoints** -- FAIL. Auto-generated summaries are just the function name in title case. Swagger UI navigation is a wall of "Get User", "Create User", "Delete User" with no context about what each does or when to use it.
- [ ] **Realistic examples provided** -- FAIL. No examples at all.
- [ ] **Error responses include problem detail** -- FAIL. No error schemas defined.
- [ ] **Common use cases covered** -- FAIL. No description of workflows or "getting started" guidance in the spec.

---

## Prioritized Remediation Plan

### Priority 1: High Impact, Low Effort

These changes improve the spec the most for the least code:

**1a. Add `title`, `description`, `version`, and `servers` to the FastAPI constructor:**

```python
app = FastAPI(
    title="Your Service Name",
    description="""
Your Service provides management of [resources].

## Authentication
All endpoints require a Bearer token unless marked as public.

## Rate Limits
- 100 requests per minute per API key
- 429 responses include `Retry-After` header

## Pagination
List endpoints accept `page` and `limit` query parameters and return a
`pagination` object with `total`, `page`, `limit`, and `pages` fields.
""",
    version="1.0.0",
    servers=[
        {"url": "https://api.yourservice.com/v1", "description": "Production"},
        {"url": "https://staging-api.yourservice.com/v1", "description": "Staging"},
    ],
)
```

**1b. Add `tags` to organize endpoints in Swagger UI:**

```python
app = FastAPI(
    # ... other params
    openapi_tags=[
        {"name": "Users", "description": "User account management"},
        {"name": "Orders", "description": "Order processing and fulfillment"},
        {"name": "Admin", "description": "Administrative operations (requires admin role)"},
    ],
)

@app.get("/users", tags=["Users"])
```

**1c. Add `summary` and `description` to every route decorator:**

```python
@app.get(
    "/users",
    summary="List users with filtering and pagination",
    description="Retrieve a paginated list of users. Supports filtering by status, "
                "role, and creation date. Results are sorted by creation date descending "
                "by default. Requires `read:users` scope.",
    tags=["Users"],
)
```

### Priority 2: High Impact, Medium Effort

**2a. Add `responses` to every endpoint:**

```python
@app.get(
    "/users/{user_id}",
    summary="Get user by ID",
    responses={
        404: {"model": ErrorResponse, "description": "User not found"},
        401: {"model": ErrorResponse, "description": "Authentication required"},
    },
)
```

**2b. Add `Field()` descriptions and examples to all Pydantic models:**

```python
from pydantic import BaseModel, Field

class User(BaseModel):
    id: str = Field(..., description="Unique user identifier", examples=["usr_abc123"])
    email: str = Field(..., description="User email address", examples=["alice@example.com"])
    name: str = Field(..., description="Display name", examples=["Alice Zhang"])
    status: str = Field(
        default="active",
        description="Account status",
        examples=["active"],
        json_schema_extra={"enum": ["active", "inactive", "suspended"]},
    )
    created_at: datetime = Field(..., description="Account creation timestamp")
```

**2c. Add `example` to query parameters:**

```python
from fastapi import Query

@app.get("/users")
async def list_users(
    page: int = Query(default=1, ge=1, description="Page number", example=1),
    limit: int = Query(default=20, ge=1, le=100, description="Items per page", example=20),
    status: str | None = Query(default=None, description="Filter by account status", example="active"),
):
```

### Priority 3: Medium Impact, Medium Effort

**3a. Define security schemes** (see Step 5 above).

**3b. Add a standardized pagination response model:**

```python
class PaginationMeta(BaseModel):
    total: int = Field(..., description="Total number of items", examples=[142])
    page: int = Field(..., description="Current page number", examples=[1])
    limit: int = Field(..., description="Items per page", examples=[20])
    pages: int = Field(..., description="Total number of pages", examples=[8])

class PaginatedResponse(BaseModel):
    data: list
    pagination: PaginationMeta
```

**3c. Use `operationId` for SDK-friendly method names:**

```python
@app.get("/users/{user_id}", operation_id="getUserById")
```

Without `operationId`, SDK generators produce method names from the function name, which may not match your desired client API.

### Priority 4: Polish

**4a. Add `externalDocs` for detailed guides:**

```python
app = FastAPI(
    # ...
    external_docs={"url": "https://docs.yourservice.com", "description": "Full documentation"},
)
```

**4b. Add `deprecated` flag to sunset endpoints:**

```python
@app.get("/v1/legacy-endpoint", deprecated=True, description="Use /v2/endpoint instead. Removal date: 2026-06-01.")
```

**4c. Document rate limit headers as response headers:**

```python
@app.get(
    "/users",
    responses={
        200: {
            "description": "Successful response",
            "headers": {
                "X-RateLimit-Limit": {"description": "Request limit per minute", "schema": {"type": "integer"}},
                "X-RateLimit-Remaining": {"description": "Remaining requests in window", "schema": {"type": "integer"}},
                "X-RateLimit-Reset": {"description": "Unix timestamp when the window resets", "schema": {"type": "integer"}},
            },
        },
        429: {"model": ErrorResponse, "description": "Rate limit exceeded. Check Retry-After header."},
    },
)
```

---

## Summary Scorecard

| Category | Score | Notes |
|----------|-------|-------|
| Completeness | 2/10 | Paths are present but everything else is at defaults |
| Accuracy | 6/10 | Pydantic-derived schemas are correct; error cases undocumented |
| Usability | 1/10 | No descriptions, no examples, no auth docs, no error docs |
| **Overall** | **3/10** | The spec is structurally valid but not usable as documentation |

After implementing Priority 1 and 2 items above (estimated 2-4 hours for 20 endpoints), the score should reach 7-8/10. Adding Priority 3 and 4 brings it to 9-10/10.

---

## FastAPI-Specific Tips

1. **Use docstrings as descriptions.** FastAPI uses the route function's docstring as the endpoint `description` field. This is the lowest-friction way to add descriptions -- just write docstrings.

2. **Use `response_model_exclude_unset=True`** to keep response examples clean when PATCH endpoints return partial objects.

3. **Use `Annotated` types for reusable parameter definitions:**

```python
from typing import Annotated
from fastapi import Query

PageParam = Annotated[int, Query(ge=1, default=1, description="Page number", example=1)]
LimitParam = Annotated[int, Query(ge=1, le=100, default=20, description="Items per page", example=20)]

@app.get("/users")
async def list_users(page: PageParam, limit: LimitParam):
    ...
```

4. **Validate your spec** after customization: visit `/docs` (Swagger UI) and `/redoc` (ReDoc) to confirm rendering. Also run `openapi-spec-validator` against `/openapi.json` to catch structural issues.

5. **Use `model_config` in Pydantic v2 for schema-level examples:**

```python
class CreateUserRequest(BaseModel):
    email: str
    name: str

    model_config = {
        "json_schema_extra": {
            "examples": [
                {"email": "alice@example.com", "name": "Alice Zhang"},
                {"email": "bob@example.com", "name": "Bob Smith"},
            ]
        }
    }
```

---

*Review generated using the API Documentation skill review workflow and quality checklist.*
