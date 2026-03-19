---
name: api-documentation
description: >-
  Patterns and templates for creating comprehensive API documentation using OpenAPI/Swagger,
  AsyncAPI, and manual documentation approaches. Use this skill whenever the user wants to
  document REST endpoints, generate or improve an OpenAPI spec, create Swagger docs, document
  WebSocket/event-driven APIs with AsyncAPI, review API documentation completeness, add
  request/response examples, document authentication schemes, set up API documentation
  portals (Swagger UI, ReDoc, Stoplight), or convert between API doc formats. Also applies
  when discussing endpoint parameters, response codes, schema definitions, pagination patterns,
  error response formats, or code-first vs design-first API documentation. If the user
  mentions "API docs", "OpenAPI", "Swagger", "AsyncAPI", "endpoint documentation",
  "API reference", or "API spec", this skill likely applies.
---

# API Documentation

API documentation is the contract between your service and its consumers. Incomplete or inaccurate API docs cause integration failures, support tickets, and abandoned adoption. This skill covers REST (OpenAPI), event-driven (AsyncAPI), and language-native API documentation patterns.

## Review Workflow

When reviewing existing API documentation, follow this sequence:

1. **Inventory endpoints** — scan route handlers/controllers and cross-reference against the spec's `paths`. Missing endpoints are the most common defect.
2. **Check required elements** — for each endpoint, verify all 7 elements below (summary, description, parameters, request body, responses, examples, auth).
3. **Validate examples** — request/response examples should be valid JSON matching the declared schemas. Invalid examples break SDK generators and mislead consumers.
4. **Audit error responses** — every endpoint should document at least 400, 404 (for resource endpoints), and 500 responses. Missing error docs force consumers to discover failures at runtime.
5. **Verify authentication** — security schemes should be defined in `components/securitySchemes` and applied globally or per-endpoint.
6. **Check the quality checklist** at the end of this document for a final pass.

## Quick Reference

For copy-paste endpoint templates, see **`references/endpoint-templates.md`**. For advanced OpenAPI patterns (discriminators, webhooks, callbacks), see **`references/openapi-patterns.md`**. For complete working specs, see `examples/petstore-openapi.yaml` and `examples/events-asyncapi.yaml`.

## OpenAPI/Swagger Overview

OpenAPI Specification (OAS) is the standard for describing REST APIs. Use it for:
- Auto-generating documentation portals
- Client SDK generation
- API testing and validation
- Contract-first development

### Supported Versions
- **OpenAPI 3.1** - Current standard, JSON Schema compatible
- **OpenAPI 3.0** - Widely supported, stable
- **Swagger 2.0** - Legacy, migrate when feasible since tooling is converging on 3.x

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

Define reusable schemas in `components/schemas` to avoid duplication. Every resource should have separate Create, Update, and Response models — consumers need different fields for each operation. See `references/endpoint-templates.md` for complete schema examples.

### Common Patterns

Every API needs these three reusable patterns. Without them, consumers reinvent pagination, error handling, and auth on every integration.

- **PaginatedResponse** — standardized wrapper with `data` array + `pagination` object (total, page, limit). Define once, reuse via `$ref` on all list endpoints.
- **Error** — standardized error body with `code` (machine-readable), `message` (human-readable), and optional `details` array. Apply to all 4xx/5xx responses.
- **Rate Limit Headers** — document `X-RateLimit-Limit`, `X-RateLimit-Remaining`, `X-RateLimit-Reset` headers and the `429 Too Many Requests` response. Consumers cannot implement retry/backoff without this.

See `references/endpoint-templates.md` for the full YAML definitions of each pattern.

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

### Completeness (missing items cause integration failures)
- [ ] All endpoints documented — cross-reference route handlers against spec paths
- [ ] All parameters described — undocumented params force consumers to read source code
- [ ] All response codes listed — missing error codes (4xx, 5xx) hide failure modes
- [ ] Authentication explained — consumers cannot make authenticated calls without this
- [ ] Rate limits documented — consumers cannot implement retry logic without knowing limits
- [ ] Pagination pattern documented — consumers need to know how to traverse large result sets

### Accuracy (inaccurate docs are worse than missing docs)
- [ ] Schemas match actual responses — validate against live API output
- [ ] Examples are valid JSON — invalid examples break SDK generators
- [ ] Status codes are correct — wrong codes cause incorrect error handling in clients
- [ ] Parameter types are accurate — type mismatches cause silent failures

### Usability (determines adoption speed)
- [ ] Clear summaries for endpoints — one-line descriptions visible in Swagger UI navigation
- [ ] Realistic examples provided — copy-paste-ready, not placeholder values
- [ ] Error responses include problem detail — help consumers diagnose and fix issues
- [ ] Common use cases covered — top 3-5 workflows should be immediately discoverable

## Generating Documentation

Choose code-first or design-first based on your team's workflow. Code-first works well when the implementation drives the API shape; design-first works when the contract needs agreement before coding starts.

### From Code (Code-First)
- Extract from decorators/annotations
- Generate from type definitions
- Tools: swagger-jsdoc, FastAPI, NestJS Swagger

### From Spec (Design-First)
- Write OpenAPI spec first, then generate code
- Generate server stubs and client SDKs
- Tools: Swagger Codegen, OpenAPI Generator

### From Language Doc Toolchains

When a project uses a language-native doc toolchain, API documentation may come from source code comments rather than OpenAPI specs. Detect and support:

| Language | Toolchain | Build Command | Config Key |
|----------|-----------|---------------|------------|
| Rust | rustdoc | `cargo doc --no-deps --all-features` | `api_docs.rustdoc` |
| Go | godoc / pkgsite | `go doc ./...` | `api_docs.godoc` |
| Python | Sphinx autodoc / pdoc | `sphinx-build` / `pdoc` | `api_docs.sphinx` |
| TypeScript | TypeDoc | `npx typedoc` | `api_docs.typedoc` |
| Java | Javadoc | `javadoc` / `./gradlew javadoc` | `api_docs.javadoc` |
| Kotlin | Dokka (KDoc) | `./gradlew dokkaHtml` | `api_docs.dokka` |
| Swift | DocC | `swift package generate-documentation` | `api_docs.docc` |
| C# | DocFX / XML docs | `docfx build` | `api_docs.docfx` |
| Elixir | ExDoc | `mix docs` | `api_docs.exdoc` |

**When reviewing a project with both OpenAPI and a language doc toolchain:**
- OpenAPI/AsyncAPI = external API contract (HTTP/event surface)
- Language doc toolchain = internal/library API surface (code-level)
- Both should be reviewed — they document different audiences
- Cross-check: HTTP endpoint docs should align with handler function doc comments

See the `documentation-standards` skill for detailed per-language doc comment conventions and review criteria.

### Documentation Portals
- **Swagger UI** - Interactive API explorer
- **ReDoc** - Clean reference documentation
- **Stoplight** - Collaborative API design
- **Astro Starlight** - Modern documentation sites with component islands

## Additional Resources

### Reference Files

For detailed patterns, consult:
- **`references/openapi-patterns.md`** - Advanced OpenAPI patterns
- **`references/endpoint-templates.md`** - Copy-paste endpoint templates

### Example Files

Working examples in `examples/`:
- **`petstore-openapi.yaml`** - Complete OpenAPI example
- **`events-asyncapi.yaml`** - AsyncAPI example
