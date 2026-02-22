---
title: "How to Create Documentation from Your Codebase"
diataxis_type: how-to
---

# How to Create Documentation from Your Codebase

This guide covers generating different types of documentation from your project using the `/doc-create` command.

## Prerequisites

- The documentation-review plugin installed (`claude plugin install zircote/documentation-review`)
- A project with source code or API specifications

## Generate a README

If you need a README for your project, run:

```
/doc-create readme
```

The plugin analyzes your project structure -- package manifests, source directories, entry points, dependencies -- and generates a `README.md` with sections for installation, usage, configuration, and more.

If a `README.md` already exists, you will be asked whether to replace or merge with the existing content.

## Generate API Documentation

### From an OpenAPI or AsyncAPI specification

If your project has an OpenAPI or AsyncAPI spec, run:

```
/doc-create api
```

The plugin auto-detects specification files (`openapi.yaml`, `openapi.json`, `swagger.yaml`, `asyncapi.yaml`). If you want to point to a specific spec, configure `api_docs.openapi_path` or `api_docs.asyncapi_path` in your [project configuration](configure-project.md).

The generated documentation includes endpoints, parameters, request/response schemas, and code examples.

### From source code

If you do not have a specification file but want to generate API docs from your code, enable code-based generation in your configuration:

```yaml
api_docs:
  generate_from_code: true
```

Then run `/doc-create api`. The plugin scans your source for route definitions, handler functions, and type annotations to produce documentation.

### Using language doc toolchains

If your project uses a language-specific documentation tool (rustdoc, typedoc, godoc, sphinx, javadoc, dokka, docc, docfx, or exdoc), configure the appropriate toolchain in your project configuration. For example, for a TypeScript project:

```yaml
api_docs:
  typedoc:
    enabled: true
    build_command: "npx typedoc"
    entry_points:
      - src/index.ts
```

The plugin integrates with the toolchain output when generating API documentation.

## Generate Documentation Templates

If you need standard documentation files like contributing guides or architecture documents, run:

```
/doc-create template
```

You will be prompted to choose from available templates:

- **contributing** -- contributor guidelines, development setup, PR process
- **architecture** -- system architecture, component diagrams, design decisions
- **deployment** -- deployment procedures, environment configuration
- **troubleshooting** -- common issues and their solutions

Each template is customized with your project's details (name, language, framework) during generation.

## Diataxis Quadrant Selection

When Diataxis is enabled (the default), the plugin asks which quadrant the new document serves and applies the appropriate structure, tone, and frontmatter. The document type provides a default mapping:

| Document type | Default quadrant |
|---------------|-----------------|
| `readme` | Mixed (overview) -- flagged for later splitting |
| `api` | Reference |
| `template` | Depends on template type |

## Verify the Output

After generation, the plugin reports what was created: file path, sections, and word count. Run a review to validate the new documentation:

```
/doc-review path/to/generated-file.md
```

## Related

- [Commands reference](../reference/commands.md) -- full details on `/doc-create` arguments
- [Skills reference](../reference/skills.md) -- documentation-standards and api-documentation skills
- [Configure your project](configure-project.md) -- set up API doc paths and toolchains
