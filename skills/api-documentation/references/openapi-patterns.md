# OpenAPI Advanced Patterns

Advanced patterns and best practices for OpenAPI specifications.

## Reusable Components

### Parameter Components

Define reusable parameters:

```yaml
components:
  parameters:
    PageParam:
      name: page
      in: query
      schema:
        type: integer
        minimum: 1
        default: 1
      description: Page number for pagination

    LimitParam:
      name: limit
      in: query
      schema:
        type: integer
        minimum: 1
        maximum: 100
        default: 20
      description: Items per page

    SortParam:
      name: sort
      in: query
      schema:
        type: string
        enum: [asc, desc]
        default: desc
      description: Sort order
```

Usage:
```yaml
paths:
  /items:
    get:
      parameters:
        - $ref: '#/components/parameters/PageParam'
        - $ref: '#/components/parameters/LimitParam'
```

### Response Components

```yaml
components:
  responses:
    NotFound:
      description: Resource not found
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/Error'
          example:
            code: NOT_FOUND
            message: The requested resource was not found

    Unauthorized:
      description: Authentication required
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/Error'

    ValidationError:
      description: Request validation failed
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/ValidationError'
```

## Schema Patterns

### Polymorphism with oneOf

```yaml
components:
  schemas:
    Pet:
      oneOf:
        - $ref: '#/components/schemas/Dog'
        - $ref: '#/components/schemas/Cat'
      discriminator:
        propertyName: petType
        mapping:
          dog: '#/components/schemas/Dog'
          cat: '#/components/schemas/Cat'

    Dog:
      type: object
      required:
        - petType
        - breed
      properties:
        petType:
          type: string
        breed:
          type: string

    Cat:
      type: object
      required:
        - petType
        - indoor
      properties:
        petType:
          type: string
        indoor:
          type: boolean
```

### Composition with allOf

```yaml
components:
  schemas:
    BaseEntity:
      type: object
      properties:
        id:
          type: string
          format: uuid
        createdAt:
          type: string
          format: date-time
        updatedAt:
          type: string
          format: date-time

    User:
      allOf:
        - $ref: '#/components/schemas/BaseEntity'
        - type: object
          required:
            - email
          properties:
            email:
              type: string
              format: email
            name:
              type: string
```

### Nullable Fields

OpenAPI 3.1 (JSON Schema):
```yaml
properties:
  middleName:
    type: ['string', 'null']
```

OpenAPI 3.0:
```yaml
properties:
  middleName:
    type: string
    nullable: true
```

## Request/Response Examples

### Multiple Examples

```yaml
paths:
  /users:
    post:
      requestBody:
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/CreateUser'
            examples:
              basic:
                summary: Basic user creation
                value:
                  email: "user@example.com"
                  name: "John Doe"
              admin:
                summary: Admin user creation
                value:
                  email: "admin@example.com"
                  name: "Admin User"
                  role: "admin"
```

### Response Examples per Status

```yaml
responses:
  '200':
    description: Success
    content:
      application/json:
        examples:
          single:
            summary: Single result
            value:
              data:
                id: "123"
                name: "Item"
          multiple:
            summary: Multiple results
            value:
              data:
                - id: "123"
                  name: "Item 1"
                - id: "456"
                  name: "Item 2"
```

## Versioning Strategies

### URL Path Versioning

```yaml
servers:
  - url: https://api.example.com/v1
    description: Version 1
  - url: https://api.example.com/v2
    description: Version 2
```

### Header Versioning

```yaml
parameters:
  - name: API-Version
    in: header
    schema:
      type: string
      enum: ['2023-01', '2024-01']
    required: false
    description: API version (defaults to latest)
```

## File Uploads

### Single File

```yaml
paths:
  /upload:
    post:
      requestBody:
        content:
          multipart/form-data:
            schema:
              type: object
              properties:
                file:
                  type: string
                  format: binary
                description:
                  type: string
```

### Multiple Files

```yaml
requestBody:
  content:
    multipart/form-data:
      schema:
        type: object
        properties:
          files:
            type: array
            items:
              type: string
              format: binary
```

## Webhooks (OpenAPI 3.1)

```yaml
webhooks:
  userCreated:
    post:
      summary: User created webhook
      requestBody:
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/UserCreatedEvent'
      responses:
        '200':
          description: Webhook processed
```

## Links for HATEOAS

```yaml
paths:
  /users/{userId}:
    get:
      responses:
        '200':
          description: User found
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/User'
          links:
            GetUserOrders:
              operationId: getUserOrders
              parameters:
                userId: '$response.body#/id'
```

## Tags and Organization

```yaml
tags:
  - name: Users
    description: User management operations
    externalDocs:
      url: https://docs.example.com/users
  - name: Orders
    description: Order processing

paths:
  /users:
    get:
      tags:
        - Users
      summary: List users
```

## Callbacks for Async Operations

```yaml
paths:
  /jobs:
    post:
      summary: Start async job
      requestBody:
        content:
          application/json:
            schema:
              type: object
              properties:
                callbackUrl:
                  type: string
                  format: uri
      callbacks:
        jobComplete:
          '{$request.body#/callbackUrl}':
            post:
              requestBody:
                content:
                  application/json:
                    schema:
                      $ref: '#/components/schemas/JobResult'
              responses:
                '200':
                  description: Callback acknowledged
```

## Validation Constraints

```yaml
components:
  schemas:
    CreateOrder:
      type: object
      required:
        - items
      properties:
        items:
          type: array
          minItems: 1
          maxItems: 100
          items:
            $ref: '#/components/schemas/OrderItem'
        notes:
          type: string
          maxLength: 500
        quantity:
          type: integer
          minimum: 1
          maximum: 1000
        email:
          type: string
          format: email
          pattern: '^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$'
```

## Deprecation

```yaml
paths:
  /legacy/users:
    get:
      deprecated: true
      summary: List users (deprecated)
      description: |
        **Deprecated:** Use `/v2/users` instead.
        This endpoint will be removed on 2025-01-01.
```
