---
diataxis_type: reference
diataxis_describes: TaskManager API endpoints
---

# TaskManager API Reference

REST API for managing tasks.

## Endpoints

### `GET /tasks`

List all tasks.

**Parameters:**

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| status | query | string | No | Filter by status: `pending`, `done`, `all` (default: `all`) |
| limit | query | integer | No | Maximum results to return (default: 50, max: 200) |
| offset | query | integer | No | Number of results to skip (default: 0) |

**Response:** `200 OK`

```json
{
  "tasks": [
    {
      "id": 1,
      "title": "string",
      "done": false,
      "created_at": "2025-01-15T10:30:00Z"
    }
  ],
  "total": 1,
  "limit": 50,
  "offset": 0
}
```

---

### `POST /tasks`

Create a new task.

**Request body:**

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| title | string | Yes | Task title (1-255 characters) |
| description | string | No | Task description (max 2000 characters) |
| due_date | string | No | ISO 8601 date |

**Example:**

```json
{
  "title": "Review documentation",
  "description": "Check all API docs for accuracy",
  "due_date": "2025-02-01T00:00:00Z"
}
```

**Response:** `201 Created`

```json
{
  "id": 2,
  "title": "Review documentation",
  "description": "Check all API docs for accuracy",
  "due_date": "2025-02-01T00:00:00Z",
  "done": false,
  "created_at": "2025-01-15T10:35:00Z"
}
```

**Errors:**

| Status | Code | Description |
|--------|------|-------------|
| 400 | `INVALID_TITLE` | Title is missing or exceeds 255 characters |
| 400 | `INVALID_DATE` | due_date is not valid ISO 8601 |

---

### `PATCH /tasks/:id`

Update an existing task.

**Path parameters:**

| Name | Type | Description |
|------|------|-------------|
| id | integer | Task ID |

**Request body:**

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| title | string | No | New task title |
| description | string | No | New task description |
| done | boolean | No | Mark task as done or not done |

**Response:** `200 OK` — Returns the updated task object.

**Errors:**

| Status | Code | Description |
|--------|------|-------------|
| 404 | `NOT_FOUND` | No task with the specified ID |
| 400 | `INVALID_TITLE` | Title exceeds 255 characters |

---

### `DELETE /tasks/:id`

Delete a task.

**Path parameters:**

| Name | Type | Description |
|------|------|-------------|
| id | integer | Task ID |

**Response:** `204 No Content`

**Errors:**

| Status | Code | Description |
|--------|------|-------------|
| 404 | `NOT_FOUND` | No task with the specified ID |

## Error Response Format

All errors follow this structure:

```json
{
  "error": {
    "code": "ERROR_CODE",
    "message": "Human-readable description"
  }
}
```

## Rate Limits

| Endpoint | Limit | Window |
|----------|-------|--------|
| All endpoints | 100 requests | 15 minutes |
| `POST /tasks` | 20 requests | 15 minutes |

Exceeded limits return `429 Too Many Requests` with a `Retry-After` header.

## See also

- [How to add rate limiting](../guides/add-rate-limiting.md)
- [How to paginate results](../guides/pagination.md)
