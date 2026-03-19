# FastAPI OpenAPI Specification Review

## Overview

This review covers a typical FastAPI application with ~20 endpoints using entirely auto-generated OpenAPI output with no customization. The assessment identifies what is missing and provides concrete guidance for producing a production-quality API specification.

---

## What Auto-Generated FastAPI OpenAPI Gives You

Out of the box, FastAPI generates an OpenAPI 3.1 spec that includes:

- Route paths and HTTP methods
- Request/response model schemas derived from Pydantic models
- Query parameter names and types
- Path parameter names and types
- A Swagger UI at `/docs` and ReDoc at `/redoc`

That is a structural skeleton. It is not documentation.

---

## What Is Missing

### 1. Application-Level Metadata

**Problem:** The spec title defaults to `"FastAPI"`, version to `"0.1.0"`, and description is empty. Consumers see no indication of what the API does, who owns it, or how to get help.

**Fix:** Customize the `FastAPI()` constructor.

```python
app = FastAPI(
    title="Inventory Management API",
    description=(
        "Manages warehouse inventory, stock levels, and order fulfillment. "
        "Authenticated via OAuth2 bearer tokens issued by the corporate IdP."
    ),
    version="2.4.0",
    contact={"name": "Platform Team", "email": "platform@example.com"},
    license_info={"name": "Proprietary"},
    terms_of_service="https://example.com/terms",
)
```

### 2. Endpoint Summaries and Descriptions

**Problem:** Every endpoint shows only its function name converted to a title (e.g., `Get Items`). There is no explanation of behavior, business rules, side effects, or expected usage.

**Fix:** Add `summary` and `description` to every route decorator.

```python
@app.get(
    "/items/{item_id}",
    summary="Retrieve a single inventory item",
    description=(
        "Returns the full item record including current stock level, "
        "warehouse location, and reorder threshold. Returns 404 if the "
        "item ID does not exist or has been soft-deleted."
    ),
)
```

### 3. Tag Organization

**Problem:** All 20 endpoints appear in a single flat list. Consumers must scroll through every endpoint to find what they need.

**Fix:** Group endpoints by domain using `tags`.

```python
@app.get("/items/", tags=["Inventory"])
@app.post("/orders/", tags=["Orders"])
@app.get("/warehouses/", tags=["Warehouses"])
```

Add tag metadata at the app level for ordering and descriptions:

```python
app = FastAPI(
    openapi_tags=[
        {"name": "Inventory", "description": "CRUD operations for inventory items"},
        {"name": "Orders", "description": "Order creation, tracking, and fulfillment"},
        {"name": "Warehouses", "description": "Warehouse configuration and capacity"},
    ]
)
```

### 4. Response Model Documentation

**Problem:** Response schemas exist but field descriptions are empty. Consumers see field names and types but not what the values mean, their units, constraints, or valid ranges.

**Fix:** Add `Field()` metadata to every Pydantic model field.

```python
from pydantic import BaseModel, Field

class Item(BaseModel):
    id: int = Field(..., description="Unique item identifier", examples=[42])
    name: str = Field(
        ...,
        description="Human-readable item name",
        min_length=1,
        max_length=200,
        examples=["Widget A"],
    )
    stock_level: int = Field(
        ...,
        description="Current units in stock across all warehouses",
        ge=0,
        examples=[150],
    )
    reorder_threshold: int = Field(
        ...,
        description="Stock level that triggers automatic reorder. Set to 0 to disable.",
        ge=0,
        examples=[25],
    )
```

### 5. Request Body Examples

**Problem:** Request bodies show the schema but no realistic example payloads. Consumers must guess at valid combinations of fields.

**Fix:** Use `model_config` with `json_schema_extra` or per-field `examples`.

```python
class ItemCreate(BaseModel):
    model_config = {
        "json_schema_extra": {
            "examples": [
                {
                    "name": "Widget A",
                    "sku": "WDG-001",
                    "stock_level": 100,
                    "reorder_threshold": 25,
                    "warehouse_id": 3,
                }
            ]
        }
    }
```

### 6. Error Response Documentation

**Problem:** Only the success response (200/201) is documented. Consumers do not know what error shapes to expect, what status codes are possible, or what the error body looks like.

**Fix:** Declare all relevant responses on each endpoint.

```python
@app.get(
    "/items/{item_id}",
    responses={
        200: {"description": "Item found and returned"},
        404: {
            "description": "Item not found or soft-deleted",
            "content": {
                "application/json": {
                    "example": {"detail": "Item 42 not found"}
                }
            },
        },
        422: {"description": "Validation error — item_id must be a positive integer"},
    },
)
```

### 7. Authentication / Security Scheme

**Problem:** If endpoints require authentication, the auto-generated spec does not reflect this unless security dependencies are explicitly declared. Consumers cannot tell which endpoints are public vs. protected.

**Fix:** Define the security scheme and apply it.

```python
from fastapi.security import OAuth2PasswordBearer

oauth2_scheme = OAuth2PasswordBearer(tokenUrl="/auth/token")

@app.get("/items/", dependencies=[Depends(oauth2_scheme)])
```

For API key auth:

```python
from fastapi.security import APIKeyHeader

api_key_header = APIKeyHeader(name="X-API-Key")
```

### 8. Deprecation Markers

**Problem:** If any of the 20 endpoints are being phased out, consumers have no way to know.

**Fix:** Mark deprecated endpoints explicitly.

```python
@app.get("/items/legacy", deprecated=True, summary="[Deprecated] Legacy item list")
```

### 9. Operation IDs

**Problem:** Auto-generated operation IDs are derived from function names (e.g., `get_items_items__get`). These are ugly, unstable, and break client code generators.

**Fix:** Set explicit `operation_id` values.

```python
@app.get("/items/", operation_id="listItems")
@app.get("/items/{item_id}", operation_id="getItem")
@app.post("/items/", operation_id="createItem")
```

### 10. Query Parameter Descriptions and Constraints

**Problem:** Query parameters show name and type but no description, default value explanation, or valid ranges.

**Fix:** Use `Query()` with full metadata.

```python
from fastapi import Query

@app.get("/items/")
async def list_items(
    page: int = Query(1, ge=1, description="Page number (1-indexed)"),
    page_size: int = Query(
        20, ge=1, le=100, description="Items per page, max 100"
    ),
    warehouse_id: int | None = Query(
        None, description="Filter by warehouse. Omit for all warehouses."
    ),
):
```

### 11. Path Parameter Descriptions

**Problem:** Path parameters are typed but not described.

**Fix:** Use `Path()` with metadata.

```python
from fastapi import Path

@app.get("/items/{item_id}")
async def get_item(
    item_id: int = Path(..., description="Unique item identifier", ge=1, examples=[42]),
):
```

### 12. Response Status Code Declarations

**Problem:** POST endpoints default to 200 even when 201 is semantically correct. DELETE endpoints do not declare 204.

**Fix:** Set the correct `status_code`.

```python
@app.post("/items/", status_code=201, summary="Create a new item")
@app.delete("/items/{item_id}", status_code=204, summary="Delete an item")
```

---

## Prioritized Action Plan

| Priority | Action | Impact |
|----------|--------|--------|
| 1 | Add app-level metadata (title, description, version, contact) | Immediate — sets context for every consumer |
| 2 | Add tags and tag metadata to group endpoints | High — makes the spec navigable |
| 3 | Add summary + description to every route | High — explains what each endpoint does |
| 4 | Add Field descriptions and examples to all Pydantic models | High — makes request/response schemas self-documenting |
| 5 | Declare error responses (4xx, 5xx) on every endpoint | Medium — prevents consumer guesswork |
| 6 | Set explicit operation IDs | Medium — stabilizes generated client code |
| 7 | Add Query/Path parameter descriptions and constraints | Medium — completes parameter documentation |
| 8 | Define security schemes | Medium — documents auth requirements |
| 9 | Set correct status codes (201, 204) | Low — semantic correctness |
| 10 | Mark deprecated endpoints | Low — relevant only if deprecations exist |

---

## Validation Checklist

After applying the changes, verify the spec by:

1. **Open `/docs`** — confirm every endpoint has a summary, description, and tag grouping
2. **Open `/redoc`** — confirm the rendered docs read like actual documentation, not just a schema dump
3. **Export the spec** (`/openapi.json`) and run it through an OpenAPI linter:
   ```bash
   npx @stoplight/spectral-cli lint openapi.json
   ```
4. **Generate a client** using `openapi-generator` or `openapi-typescript` — confirm operation IDs produce readable method names
5. **Review examples** — confirm every request body and response has at least one realistic example

---

## Summary

A FastAPI app with 20 endpoints and zero OpenAPI customization is publishing a machine-readable schema but not human-readable documentation. The auto-generated spec answers "what routes exist" but not "what do they do, how do I use them, what can go wrong, and what do the fields mean." The fixes above — app metadata, tags, descriptions, field metadata, error responses, security schemes, and operation IDs — transform the spec from a structural skeleton into a self-contained API reference that consumers can use without reading source code or asking the maintainer questions.
