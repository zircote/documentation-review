# FastAPI OpenAPI Specification Review

## Summary

A review of a typical FastAPI application with ~20 endpoints using entirely auto-generated OpenAPI output. This document identifies what is missing, why it matters, and how to improve the specification so it is useful to consumers (frontend developers, third-party integrators, generated SDK users, and your future self).

---

## 1. What Auto-Generated FastAPI OpenAPI Typically Lacks

### 1.1 App-Level Metadata

FastAPI's defaults produce an OpenAPI spec with a title of "FastAPI" and version "0.1.0". Consumers see no useful context about what the API does.

**What is missing:**
- Meaningful API title and version
- Description (supports Markdown) explaining the API's purpose
- Contact information and license
- Terms of service URL
- External documentation links
- Server URLs for different environments (staging, production)

**Fix — pass metadata to the FastAPI constructor:**

```python
from fastapi import FastAPI

app = FastAPI(
    title="Acme Inventory Service",
    description="""
Manages product inventory, stock levels, and warehouse assignments.

## Authentication
All endpoints require a Bearer token in the `Authorization` header.

## Rate Limits
- 100 requests/minute for standard keys
- 1000 requests/minute for premium keys
""",
    version="2.4.0",
    contact={
        "name": "Platform Team",
        "url": "https://acme.dev/support",
        "email": "platform@acme.dev",
    },
    license_info={
        "name": "Apache 2.0",
        "url": "https://www.apache.org/licenses/LICENSE-2.0.html",
    },
    servers=[
        {"url": "https://api.acme.dev", "description": "Production"},
        {"url": "https://api.staging.acme.dev", "description": "Staging"},
    ],
)
```

---

### 1.2 Endpoint Summaries and Descriptions

By default, FastAPI derives the operation summary from the function name (e.g., `read_items` becomes "Read Items"). There is no description, and the summary is often too terse to be useful.

**What is missing per endpoint:**
- Human-readable summary (short, shown in sidebar of docs UIs)
- Description (longer, supports Markdown, shown when expanded)
- Deprecated flag for endpoints being phased out

**Fix — add `summary`, `description`, and `deprecated` to each route decorator:**

```python
@app.get(
    "/items/{item_id}",
    summary="Get a single inventory item",
    description="""
Retrieve full details for an inventory item by its UUID.

Returns warehouse assignment, current stock level, and reorder threshold.
Requires `inventory:read` scope.
""",
    deprecated=False,
)
async def get_item(item_id: uuid.UUID):
    ...
```

Alternatively, the docstring of the function is used as the description if no explicit `description` is provided. Pick one convention and use it consistently across all 20 endpoints.

---

### 1.3 Response Model Documentation

Auto-generated specs include the response schema shape but lack:
- Human-readable descriptions on the response itself
- Documentation of non-200 responses (4xx, 5xx)
- Example values

**What is missing:**
- `response_description` on every endpoint
- `responses` dict covering error codes (400, 401, 403, 404, 409, 422, 500)
- Realistic `example` or `examples` on response models

**Fix — declare responses explicitly:**

```python
@app.get(
    "/items/{item_id}",
    response_model=ItemOut,
    response_description="The requested inventory item",
    responses={
        404: {
            "description": "Item not found",
            "content": {
                "application/json": {
                    "example": {"detail": "Item abc-123 not found"}
                }
            },
        },
        403: {"description": "Insufficient permissions"},
    },
)
async def get_item(item_id: uuid.UUID):
    ...
```

---

### 1.4 Request Body and Parameter Documentation

Pydantic models auto-generate schemas, but field descriptions and examples are typically absent.

**What is missing:**
- `Field(description=...)` on every Pydantic model field
- `Field(example=...)` or `json_schema_extra` with realistic values
- `Query(description=...)`, `Path(description=...)`, `Header(description=...)` on all parameters
- Constraints documentation (min/max length, regex patterns, enums)

**Fix — annotate Pydantic models and parameters:**

```python
from pydantic import BaseModel, Field

class ItemCreate(BaseModel):
    """Request body for creating a new inventory item."""

    name: str = Field(
        ...,
        description="Human-readable product name",
        min_length=1,
        max_length=200,
        examples=["Widget Pro 3000"],
    )
    sku: str = Field(
        ...,
        description="Stock-keeping unit, unique across the catalog",
        pattern=r"^[A-Z]{2,4}-\d{4,8}$",
        examples=["WP-00042"],
    )
    quantity: int = Field(
        ...,
        description="Initial stock quantity",
        ge=0,
        examples=[150],
    )
```

For path and query parameters:

```python
from fastapi import Path, Query

@app.get("/items/")
async def list_items(
    warehouse_id: uuid.UUID = Query(
        ...,
        description="Filter items by warehouse UUID",
    ),
    page: int = Query(
        1,
        ge=1,
        description="Page number for paginated results",
    ),
    page_size: int = Query(
        20,
        ge=1,
        le=100,
        description="Number of items per page",
    ),
):
    ...
```

---

### 1.5 Tags and Grouping

With 20 endpoints and no tags, the docs UI displays a flat, unsorted list. This makes navigation difficult.

**What is missing:**
- `tags` on every route
- `openapi_tags` metadata with descriptions for each tag group

**Fix — define tag metadata and assign tags:**

```python
tags_metadata = [
    {
        "name": "Items",
        "description": "CRUD operations for inventory items",
    },
    {
        "name": "Warehouses",
        "description": "Manage warehouse locations and capacity",
    },
    {
        "name": "Reports",
        "description": "Stock reports and analytics",
    },
    {
        "name": "Admin",
        "description": "Administrative operations. Requires `admin` role.",
    },
]

app = FastAPI(
    title="Acme Inventory Service",
    openapi_tags=tags_metadata,
    # ... other metadata
)

@app.get("/items/", tags=["Items"])
async def list_items():
    ...

@app.post("/items/", tags=["Items"])
async def create_item():
    ...

@app.get("/warehouses/", tags=["Warehouses"])
async def list_warehouses():
    ...
```

---

### 1.6 Security Scheme Documentation

If the app uses authentication, the auto-generated spec likely lacks security scheme definitions, meaning consumers cannot see how to authenticate.

**What is missing:**
- Global or per-endpoint security scheme declarations
- Description of authentication flows

**Fix — declare security schemes:**

```python
from fastapi.security import HTTPBearer, OAuth2PasswordBearer

# For simple Bearer token:
security_scheme = HTTPBearer(
    scheme_name="BearerAuth",
    description="JWT token obtained from /auth/token",
)

# For OAuth2:
oauth2_scheme = OAuth2PasswordBearer(
    tokenUrl="/auth/token",
    scopes={
        "inventory:read": "Read inventory items",
        "inventory:write": "Create and update inventory items",
        "admin": "Full administrative access",
    },
)
```

---

### 1.7 Operation IDs

Auto-generated operation IDs are derived from function names and can be inconsistent. These IDs are used by SDK generators to name client methods.

**What is missing:**
- Explicit, consistent `operation_id` values following a naming convention

**Fix — set operation IDs explicitly:**

```python
@app.get("/items/{item_id}", operation_id="getItem")
async def get_item(item_id: uuid.UUID):
    ...

@app.get("/items/", operation_id="listItems")
async def list_items():
    ...

@app.post("/items/", operation_id="createItem")
async def create_item(body: ItemCreate):
    ...
```

Use a consistent convention such as `verbNoun` (camelCase) across all endpoints. This produces clean SDK method names when consumers generate clients.

---

### 1.8 Schema Examples (request/response pairs)

The auto-generated spec has no request/response examples, making the "Try it out" feature in Swagger UI less useful and requiring consumers to guess at valid payloads.

**Fix — add `model_config` with `json_schema_extra` on Pydantic models:**

```python
class ItemCreate(BaseModel):
    name: str
    sku: str
    quantity: int

    model_config = {
        "json_schema_extra": {
            "examples": [
                {
                    "name": "Widget Pro 3000",
                    "sku": "WP-00042",
                    "quantity": 150,
                }
            ]
        }
    }
```

---

## 2. Checklist for All 20 Endpoints

Apply this checklist to every endpoint in `src/main.py`:

| # | Item | Status |
|---|------|--------|
| 1 | Route has `summary` (or a clean function-name-derived one) | |
| 2 | Route has `description` (or docstring) explaining behavior | |
| 3 | Route has `tags` for grouping | |
| 4 | Route has explicit `operation_id` | |
| 5 | Route has `response_model` with described fields | |
| 6 | Route has `response_description` | |
| 7 | Route declares error `responses` (at least 400, 404, 422) | |
| 8 | All path parameters use `Path(description=...)` | |
| 9 | All query parameters use `Query(description=...)` | |
| 10 | All request body models have `Field(description=..., examples=...)` | |
| 11 | Deprecated endpoints are marked `deprecated=True` | |
| 12 | Security dependencies are applied where needed | |

---

## 3. Structural Recommendations

### 3.1 Use a Router-per-Domain Pattern

With 20 endpoints, split them into `APIRouter` instances by domain. Each router gets its own `prefix` and `tags`:

```python
# src/routers/items.py
from fastapi import APIRouter

router = APIRouter(prefix="/items", tags=["Items"])

@router.get("/", summary="List inventory items")
async def list_items():
    ...
```

```python
# src/main.py
from src.routers import items, warehouses, reports, admin

app.include_router(items.router)
app.include_router(warehouses.router)
app.include_router(reports.router)
app.include_router(admin.router)
```

### 3.2 Centralize Error Response Models

Define a shared error model and reference it in `responses`:

```python
class ErrorResponse(BaseModel):
    """Standard error response body."""
    detail: str = Field(description="Human-readable error message")
    code: str | None = Field(None, description="Machine-readable error code")

# Reusable response definitions
COMMON_RESPONSES = {
    401: {"model": ErrorResponse, "description": "Authentication required"},
    403: {"model": ErrorResponse, "description": "Insufficient permissions"},
    422: {"description": "Validation error (see body for details)"},
    500: {"model": ErrorResponse, "description": "Internal server error"},
}
```

### 3.3 Export and Validate the Spec

Add a script or CI step to export and lint the generated spec:

```bash
# Export the spec
python -c "import json; from src.main import app; print(json.dumps(app.openapi(), indent=2))" > openapi.json

# Validate with spectral or similar
npx @stoplight/spectral-cli lint openapi.json
```

This catches missing descriptions, undocumented responses, and schema issues before they reach consumers.

---

## 4. Priority Order for a 20-Endpoint App

If you cannot do everything at once, prioritize in this order:

1. **App metadata** (title, description, version) — 5 minutes, immediate improvement
2. **Tags and grouping** — 10 minutes, makes docs navigable
3. **Endpoint summaries and descriptions** — 30 minutes, biggest consumer impact
4. **Pydantic field descriptions and examples** — 45 minutes, makes schemas self-documenting
5. **Error responses** — 30 minutes, critical for client developers
6. **Security schemes** — 15 minutes, required for authenticated APIs
7. **Operation IDs** — 15 minutes, important if consumers generate SDKs
8. **Request/response examples** — 20 minutes, polish for "Try it out" experience

**Estimated total effort: 2-3 hours for a 20-endpoint app.**

---

## 5. Before and After Comparison

### Before (auto-generated default)
```json
{
  "openapi": "3.1.0",
  "info": { "title": "FastAPI", "version": "0.1.0" },
  "paths": {
    "/items/{item_id}": {
      "get": {
        "summary": "Get Item",
        "operationId": "get_item_items__item_id__get",
        "parameters": [
          { "name": "item_id", "in": "path", "required": true,
            "schema": { "type": "string" } }
        ],
        "responses": {
          "200": { "description": "Successful Response" },
          "422": { "description": "Validation Error" }
        }
      }
    }
  }
}
```

### After (customized)
```json
{
  "openapi": "3.1.0",
  "info": {
    "title": "Acme Inventory Service",
    "description": "Manages product inventory, stock levels, and warehouse assignments.",
    "version": "2.4.0",
    "contact": { "name": "Platform Team", "email": "platform@acme.dev" }
  },
  "paths": {
    "/items/{item_id}": {
      "get": {
        "tags": ["Items"],
        "summary": "Get a single inventory item",
        "description": "Retrieve full details for an inventory item by its UUID.\nReturns warehouse assignment, current stock level, and reorder threshold.\nRequires `inventory:read` scope.",
        "operationId": "getItem",
        "parameters": [
          { "name": "item_id", "in": "path", "required": true,
            "schema": { "type": "string", "format": "uuid" },
            "description": "Unique identifier of the inventory item" }
        ],
        "responses": {
          "200": {
            "description": "The requested inventory item",
            "content": { "application/json": { "schema": { "$ref": "#/components/schemas/ItemOut" } } }
          },
          "404": {
            "description": "Item not found",
            "content": { "application/json": { "example": { "detail": "Item abc-123 not found" } } }
          },
          "401": { "description": "Authentication required" },
          "403": { "description": "Insufficient permissions" }
        },
        "security": [{ "BearerAuth": ["inventory:read"] }]
      }
    }
  }
}
```

The difference is stark: the "after" version is self-documenting, navigable, and usable without supplementary documentation.
