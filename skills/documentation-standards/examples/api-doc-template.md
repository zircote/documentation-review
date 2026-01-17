# API Reference

Brief description of the API and its purpose.

## Authentication

Describe authentication requirements.

```bash
# Example authentication header
curl -H "Authorization: Bearer <token>" https://api.example.com/v1/resource
```

## Base URL

```
https://api.example.com/v1
```

## Rate Limits

| Tier | Requests/Minute | Burst |
|------|-----------------|-------|
| Free | 60 | 10 |
| Pro | 600 | 100 |

---

## Endpoints

### Resource Name

#### List Resources

```http
GET /resources
```

**Description:** Brief description of what this endpoint does.

**Parameters:**

| Name | Type | In | Required | Description |
|------|------|-----|----------|-------------|
| `limit` | integer | query | No | Maximum results (default: 20, max: 100) |
| `offset` | integer | query | No | Skip N results for pagination |
| `filter` | string | query | No | Filter by status |

**Response:**

```json
{
  "data": [
    {
      "id": "res-123",
      "name": "Example Resource",
      "createdAt": "2024-01-15T10:30:00Z"
    }
  ],
  "pagination": {
    "total": 100,
    "limit": 20,
    "offset": 0
  }
}
```

**Status Codes:**

| Code | Description |
|------|-------------|
| 200 | Success |
| 400 | Invalid parameters |
| 401 | Authentication required |

---

#### Get Resource

```http
GET /resources/{id}
```

**Description:** Retrieve a single resource by ID.

**Path Parameters:**

| Name | Type | Description |
|------|------|-------------|
| `id` | string | Resource unique identifier |

**Response:**

```json
{
  "id": "res-123",
  "name": "Example Resource",
  "description": "Detailed description",
  "status": "active",
  "createdAt": "2024-01-15T10:30:00Z",
  "updatedAt": "2024-01-16T14:20:00Z"
}
```

**Status Codes:**

| Code | Description |
|------|-------------|
| 200 | Success |
| 404 | Resource not found |

---

#### Create Resource

```http
POST /resources
```

**Description:** Create a new resource.

**Request Body:**

```json
{
  "name": "New Resource",
  "description": "Resource description"
}
```

**Request Fields:**

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `name` | string | Yes | Resource name (1-100 chars) |
| `description` | string | No | Optional description |

**Response:**

```json
{
  "id": "res-456",
  "name": "New Resource",
  "description": "Resource description",
  "status": "active",
  "createdAt": "2024-01-17T09:00:00Z"
}
```

**Status Codes:**

| Code | Description |
|------|-------------|
| 201 | Created successfully |
| 400 | Validation error |
| 409 | Resource already exists |

---

#### Update Resource

```http
PUT /resources/{id}
```

**Description:** Update an existing resource.

**Path Parameters:**

| Name | Type | Description |
|------|------|-------------|
| `id` | string | Resource unique identifier |

**Request Body:**

```json
{
  "name": "Updated Name",
  "description": "Updated description"
}
```

**Response:** Returns the updated resource object.

**Status Codes:**

| Code | Description |
|------|-------------|
| 200 | Updated successfully |
| 400 | Validation error |
| 404 | Resource not found |

---

#### Delete Resource

```http
DELETE /resources/{id}
```

**Description:** Delete a resource.

**Path Parameters:**

| Name | Type | Description |
|------|------|-------------|
| `id` | string | Resource unique identifier |

**Response:** No content.

**Status Codes:**

| Code | Description |
|------|-------------|
| 204 | Deleted successfully |
| 404 | Resource not found |

---

## Error Responses

All errors return a consistent format:

```json
{
  "code": "ERROR_CODE",
  "message": "Human-readable error message",
  "details": [
    {
      "field": "name",
      "message": "Name is required"
    }
  ]
}
```

### Common Error Codes

| Code | HTTP Status | Description |
|------|-------------|-------------|
| `VALIDATION_ERROR` | 400 | Invalid request data |
| `UNAUTHORIZED` | 401 | Authentication required |
| `FORBIDDEN` | 403 | Insufficient permissions |
| `NOT_FOUND` | 404 | Resource not found |
| `CONFLICT` | 409 | Resource conflict |
| `RATE_LIMITED` | 429 | Too many requests |
| `SERVER_ERROR` | 500 | Internal server error |

---

## SDKs and Libraries

- [JavaScript/TypeScript](https://github.com/example/sdk-js)
- [Python](https://github.com/example/sdk-python)
- [Go](https://github.com/example/sdk-go)

## Changelog

See CHANGELOG.md for version history.

## Support

- Documentation: https://docs.example.com
- Issues: https://github.com/example/api/issues
- Email: support@example.com
