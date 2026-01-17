# API Endpoint Templates

Copy-paste templates for common API endpoint documentation patterns.

## CRUD Operations

### List Resources (GET /resources)

```yaml
/resources:
  get:
    summary: List all resources
    description: Retrieve a paginated list of resources
    operationId: listResources
    tags:
      - Resources
    parameters:
      - name: page
        in: query
        schema:
          type: integer
          minimum: 1
          default: 1
        description: Page number
      - name: limit
        in: query
        schema:
          type: integer
          minimum: 1
          maximum: 100
          default: 20
        description: Items per page
      - name: sort
        in: query
        schema:
          type: string
          enum: [created_at, updated_at, name]
          default: created_at
        description: Field to sort by
      - name: order
        in: query
        schema:
          type: string
          enum: [asc, desc]
          default: desc
        description: Sort order
    responses:
      '200':
        description: List of resources
        content:
          application/json:
            schema:
              type: object
              properties:
                data:
                  type: array
                  items:
                    $ref: '#/components/schemas/Resource'
                pagination:
                  $ref: '#/components/schemas/Pagination'
      '401':
        $ref: '#/components/responses/Unauthorized'
```

### Get Single Resource (GET /resources/{id})

```yaml
/resources/{resourceId}:
  get:
    summary: Get a resource by ID
    description: Retrieve a single resource by its unique identifier
    operationId: getResource
    tags:
      - Resources
    parameters:
      - name: resourceId
        in: path
        required: true
        schema:
          type: string
          format: uuid
        description: Unique resource identifier
    responses:
      '200':
        description: Resource found
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/Resource'
      '404':
        $ref: '#/components/responses/NotFound'
      '401':
        $ref: '#/components/responses/Unauthorized'
```

### Create Resource (POST /resources)

```yaml
/resources:
  post:
    summary: Create a new resource
    description: Create a new resource with the provided data
    operationId: createResource
    tags:
      - Resources
    requestBody:
      required: true
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/CreateResourceRequest'
          example:
            name: "New Resource"
            description: "Resource description"
    responses:
      '201':
        description: Resource created successfully
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/Resource'
        headers:
          Location:
            schema:
              type: string
            description: URL of the created resource
      '400':
        $ref: '#/components/responses/ValidationError'
      '401':
        $ref: '#/components/responses/Unauthorized'
      '409':
        description: Resource already exists
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/Error'
```

### Update Resource (PUT /resources/{id})

```yaml
/resources/{resourceId}:
  put:
    summary: Update a resource
    description: Replace all fields of an existing resource
    operationId: updateResource
    tags:
      - Resources
    parameters:
      - name: resourceId
        in: path
        required: true
        schema:
          type: string
          format: uuid
        description: Unique resource identifier
    requestBody:
      required: true
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/UpdateResourceRequest'
    responses:
      '200':
        description: Resource updated successfully
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/Resource'
      '400':
        $ref: '#/components/responses/ValidationError'
      '404':
        $ref: '#/components/responses/NotFound'
      '401':
        $ref: '#/components/responses/Unauthorized'
```

### Partial Update (PATCH /resources/{id})

```yaml
/resources/{resourceId}:
  patch:
    summary: Partially update a resource
    description: Update specific fields of an existing resource
    operationId: patchResource
    tags:
      - Resources
    parameters:
      - name: resourceId
        in: path
        required: true
        schema:
          type: string
          format: uuid
    requestBody:
      required: true
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/PatchResourceRequest'
          example:
            name: "Updated Name"
    responses:
      '200':
        description: Resource updated
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/Resource'
      '400':
        $ref: '#/components/responses/ValidationError'
      '404':
        $ref: '#/components/responses/NotFound'
```

### Delete Resource (DELETE /resources/{id})

```yaml
/resources/{resourceId}:
  delete:
    summary: Delete a resource
    description: Permanently delete a resource by ID
    operationId: deleteResource
    tags:
      - Resources
    parameters:
      - name: resourceId
        in: path
        required: true
        schema:
          type: string
          format: uuid
    responses:
      '204':
        description: Resource deleted successfully
      '404':
        $ref: '#/components/responses/NotFound'
      '401':
        $ref: '#/components/responses/Unauthorized'
```

## Common Schemas

### Pagination Schema

```yaml
components:
  schemas:
    Pagination:
      type: object
      properties:
        total:
          type: integer
          description: Total number of items
          example: 100
        page:
          type: integer
          description: Current page number
          example: 1
        limit:
          type: integer
          description: Items per page
          example: 20
        pages:
          type: integer
          description: Total number of pages
          example: 5
```

### Error Schema

```yaml
components:
  schemas:
    Error:
      type: object
      required:
        - code
        - message
      properties:
        code:
          type: string
          description: Machine-readable error code
          example: VALIDATION_ERROR
        message:
          type: string
          description: Human-readable error message
          example: "The request body is invalid"
        details:
          type: array
          description: Detailed error information
          items:
            type: object
            properties:
              field:
                type: string
                example: email
              message:
                type: string
                example: "Invalid email format"
```

### Timestamp Fields

```yaml
components:
  schemas:
    Timestamps:
      type: object
      properties:
        createdAt:
          type: string
          format: date-time
          readOnly: true
          description: Creation timestamp
        updatedAt:
          type: string
          format: date-time
          readOnly: true
          description: Last update timestamp
```

## Search and Filter

### Search Endpoint

```yaml
/resources/search:
  post:
    summary: Search resources
    description: Search resources with complex filters
    operationId: searchResources
    tags:
      - Resources
    requestBody:
      required: true
      content:
        application/json:
          schema:
            type: object
            properties:
              query:
                type: string
                description: Search query
              filters:
                type: object
                properties:
                  status:
                    type: array
                    items:
                      type: string
                      enum: [active, inactive, pending]
                  createdAfter:
                    type: string
                    format: date-time
                  createdBefore:
                    type: string
                    format: date-time
              page:
                type: integer
                default: 1
              limit:
                type: integer
                default: 20
    responses:
      '200':
        description: Search results
        content:
          application/json:
            schema:
              type: object
              properties:
                data:
                  type: array
                  items:
                    $ref: '#/components/schemas/Resource'
                pagination:
                  $ref: '#/components/schemas/Pagination'
```

## Bulk Operations

### Bulk Create

```yaml
/resources/bulk:
  post:
    summary: Create multiple resources
    operationId: bulkCreateResources
    requestBody:
      content:
        application/json:
          schema:
            type: object
            required:
              - items
            properties:
              items:
                type: array
                minItems: 1
                maxItems: 100
                items:
                  $ref: '#/components/schemas/CreateResourceRequest'
    responses:
      '200':
        description: Bulk operation results
        content:
          application/json:
            schema:
              type: object
              properties:
                successful:
                  type: integer
                failed:
                  type: integer
                results:
                  type: array
                  items:
                    type: object
                    properties:
                      index:
                        type: integer
                      success:
                        type: boolean
                      resource:
                        $ref: '#/components/schemas/Resource'
                      error:
                        $ref: '#/components/schemas/Error'
```

## File Operations

### Upload File

```yaml
/resources/{resourceId}/attachments:
  post:
    summary: Upload attachment
    operationId: uploadAttachment
    parameters:
      - name: resourceId
        in: path
        required: true
        schema:
          type: string
    requestBody:
      content:
        multipart/form-data:
          schema:
            type: object
            required:
              - file
            properties:
              file:
                type: string
                format: binary
              description:
                type: string
                maxLength: 500
    responses:
      '201':
        description: File uploaded
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/Attachment'
```

## Async Operations

### Start Async Job

```yaml
/jobs:
  post:
    summary: Start async job
    operationId: startJob
    requestBody:
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/JobRequest'
    responses:
      '202':
        description: Job accepted
        content:
          application/json:
            schema:
              type: object
              properties:
                jobId:
                  type: string
                  format: uuid
                status:
                  type: string
                  enum: [pending, processing]
                statusUrl:
                  type: string
                  format: uri

/jobs/{jobId}:
  get:
    summary: Get job status
    operationId: getJobStatus
    parameters:
      - name: jobId
        in: path
        required: true
        schema:
          type: string
          format: uuid
    responses:
      '200':
        description: Job status
        content:
          application/json:
            schema:
              type: object
              properties:
                jobId:
                  type: string
                status:
                  type: string
                  enum: [pending, processing, completed, failed]
                progress:
                  type: integer
                  minimum: 0
                  maximum: 100
                result:
                  type: object
                error:
                  $ref: '#/components/schemas/Error'
```
