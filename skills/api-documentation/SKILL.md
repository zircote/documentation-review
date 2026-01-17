---
name: API Documentation
description: This skill should be used when the user asks to "document an API", "create API docs", "generate OpenAPI spec", "review API documentation", "document REST endpoints", "create Swagger docs", "document AsyncAPI", "improve endpoint documentation", or needs guidance on API specification formats, endpoint documentation patterns, or API reference writing.
version: 0.1.0
---

# API Documentation

Guidance for creating comprehensive API documentation using OpenAPI/Swagger, AsyncAPI, and manual documentation patterns.

## OpenAPI/Swagger Overview

OpenAPI Specification (OAS) is the standard for describing REST APIs. Use it for:
- Auto-generating documentation portals
- Client SDK generation
- API testing and validation
- Contract-first development

### Supported Versions
- **OpenAPI 3.1** - Current standard, JSON Schema compatible
- **OpenAPI 3.0** - Widely supported, stable
- **Swagger 2.0** - Legacy, still common

### Basic Structure

```yaml
openapi: 3.1.0
info:
  title: API Name
  version: 1.0.0
  description: Brief API description
servers:
  - url: https://api.example.com/v1
paths:
  /resource:
    get:
      summary: Get resources
      responses:
        '200':
          description: Success
```

## Documenting Endpoints

### Required Elements

For each endpoint, document:

1. **HTTP Method and Path** - `GET /users/{id}`
2. **Summary** - One-line description
3. **Description** - Detailed explanation (when needed)
4. **Parameters** - Path, query, header parameters
5. **Request Body** - For POST/PUT/PATCH
6. **Responses** - All possible response codes
7. **Examples** - Request/response examples

### Path Parameters

```yaml
parameters:
  - name: userId
    in: path
    required: true
    description: Unique user identifier
    schema:
      type: string
      format: uuid
    example: "123e4567-e89b-12d3-a456-426614174000"
```

### Query Parameters

```yaml
parameters:
  - name: limit
    in: query
    required: false
    description: Maximum number of results
    schema:
      type: integer
      minimum: 1
      maximum: 100
      default: 20
```

### Request Bodies

```yaml
requestBody:
  required: true
  content:
    application/json:
      schema:
        $ref: '#/components/schemas/CreateUser'
      example:
        name: "John Doe"
        email: "john@example.com"
```

### Response Documentation

Document all response codes:

```yaml
responses:
  '200':
    description: Successful response
    content:
      application/json:
        schema:
          $ref: '#/components/schemas/User'
  '400':
    description: Invalid request parameters
    content:
      application/json:
        schema:
          $ref: '#/components/schemas/Error'
  '404':
    description: User not found
  '500':
    description: Internal server error
```

## Schema Definitions

### Component Schemas

Define reusable schemas in components:

```yaml
components:
  schemas:
    User:
      type: object
      required:
        - id
        - email
      properties:
        id:
          type: string
          format: uuid
          description: Unique identifier
        email:
          type: string
          format: email
          description: User email address
        name:
          type: string
          description: Display name
        createdAt:
          type: string
          format: date-time
```

### Common Patterns

**Pagination:**
```yaml
PaginatedResponse:
  type: object
  properties:
    data:
      type: array
      items:
        $ref: '#/components/schemas/Item'
    pagination:
      type: object
      properties:
        total:
          type: integer
        page:
          type: integer
        limit:
          type: integer
```

**Error Response:**
```yaml
Error:
  type: object
  required:
    - code
    - message
  properties:
    code:
      type: string
      description: Error code
    message:
      type: string
      description: Human-readable message
    details:
      type: array
      items:
        type: object
```

## Authentication Documentation

### Security Schemes

```yaml
components:
  securitySchemes:
    bearerAuth:
      type: http
      scheme: bearer
      bearerFormat: JWT
    apiKey:
      type: apiKey
      in: header
      name: X-API-Key
    oauth2:
      type: oauth2
      flows:
        authorizationCode:
          authorizationUrl: https://auth.example.com/authorize
          tokenUrl: https://auth.example.com/token
          scopes:
            read: Read access
            write: Write access
```

### Applying Security

```yaml
# Global security
security:
  - bearerAuth: []

# Per-endpoint override
paths:
  /public:
    get:
      security: []  # No auth required
```

## AsyncAPI for Event-Driven APIs

For message-based APIs (WebSocket, MQTT, Kafka):

```yaml
asyncapi: 2.6.0
info:
  title: Events API
  version: 1.0.0
channels:
  user/created:
    subscribe:
      summary: User created events
      message:
        $ref: '#/components/messages/UserCreated'
components:
  messages:
    UserCreated:
      payload:
        type: object
        properties:
          userId:
            type: string
          timestamp:
            type: string
            format: date-time
```

## Documentation Quality Checklist

### Completeness
- [ ] All endpoints documented
- [ ] All parameters described
- [ ] All response codes listed
- [ ] Authentication explained
- [ ] Rate limits documented

### Accuracy
- [ ] Schemas match actual responses
- [ ] Examples are valid JSON
- [ ] Status codes are correct
- [ ] Parameter types are accurate

### Usability
- [ ] Clear summaries for endpoints
- [ ] Realistic examples provided
- [ ] Error responses helpful
- [ ] Common use cases covered

## Generating Documentation

### From Code (Code-First)
- Extract from decorators/annotations
- Generate from type definitions
- Tools: swagger-jsdoc, FastAPI, NestJS Swagger

### From Spec (Design-First)
- Write OpenAPI spec first
- Generate server stubs
- Generate client SDKs
- Tools: Swagger Codegen, OpenAPI Generator

### Documentation Portals
- **Swagger UI** - Interactive API explorer
- **ReDoc** - Clean reference documentation
- **Stoplight** - Collaborative API design

## Additional Resources

### Reference Files

For detailed patterns, consult:
- **`references/openapi-patterns.md`** - Advanced OpenAPI patterns
- **`references/endpoint-templates.md`** - Copy-paste endpoint templates

### Example Files

Working examples in `examples/`:
- **`petstore-openapi.yaml`** - Complete OpenAPI example
- **`events-asyncapi.yaml`** - AsyncAPI example
