---
title: "Skills"
diataxis_type: reference
---

# Skills

Skills are knowledge modules loaded by commands and agents to provide domain-specific context. The plugin includes four skills.

## Summary

| Skill | Version | Diataxis Type | Triggers |
|-------|---------|---------------|----------|
| `documentation-standards` | 0.1.0 | reference | "review documentation", "improve docs quality", "check markdown formatting" |
| `api-documentation` | 0.1.0 | reference | "document an API", "create API docs", "generate OpenAPI spec" |
| `changelog` | 0.1.0 | how-to | "update changelog", "add changelog entry", "prepare release notes" |
| `diataxis` | 0.1.0 | reference | "apply diataxis", "classify documentation", "documentation gap analysis" |

## documentation-standards

Provides quality criteria, formatting rules, and writing conventions for technical documentation.

**Version:** 0.1.0
**Diataxis type:** reference

**Trigger phrases:** "review documentation", "improve docs quality", "check markdown formatting", "fix documentation structure", "write better docs", "documentation best practices"

**Coverage:**

- Core quality principles: clarity, completeness, accuracy, consistency
- Document structure and heading hierarchy
- Markdown best practices
- Review checklist and common issues
- Writing style guidelines
- Language doc toolchains: Rust (rustdoc), Go (godoc), Python (Sphinx), TypeScript (TypeDoc), Java (Javadoc), Kotlin (Dokka), Swift (DocC), C# (DocFX), Elixir (ExDoc)
- Site generator integration: MkDocs, Sphinx, Docusaurus, Astro

**Bundled references:**

| File | Description |
|------|-------------|
| `references/style-guide.md` | Writing style conventions |
| `references/review-criteria.md` | Scoring criteria for reviews |

**Bundled examples:**

| File | Description |
|------|-------------|
| `examples/good-readme.md` | Model README structure |
| `examples/api-doc-template.md` | API documentation template |

## api-documentation

Provides patterns and templates for API specification formats and endpoint documentation.

**Version:** 0.1.0
**Diataxis type:** reference

**Trigger phrases:** "document an API", "create API docs", "generate OpenAPI spec", "review API documentation", "document REST endpoints", "create Swagger docs", "document AsyncAPI"

**Coverage:**

- OpenAPI / Swagger (3.0, 3.1)
- AsyncAPI
- Endpoint documentation patterns
- Schema definitions
- Authentication documentation
- Language doc toolchains

**Bundled references:**

| File | Description |
|------|-------------|
| `references/openapi-patterns.md` | Common OpenAPI patterns |
| `references/endpoint-templates.md` | Endpoint documentation templates |

**Bundled examples:**

| File | Description |
|------|-------------|
| `examples/petstore-openapi.yaml` | Petstore OpenAPI spec |
| `examples/events-asyncapi.yaml` | Events AsyncAPI spec |

## changelog

Provides guidance on changelog management, conventional commits, and release workflows.

**Version:** 0.1.0
**Diataxis type:** how-to

**Trigger phrases:** "update changelog", "add changelog entry", "prepare release notes", "document version changes", "generate changelog", "review release history", "create release entry"

**Coverage:**

- Keep a Changelog format
- semantic-release configuration
- Conventional commits mapping (commit types to changelog sections)
- GitHub Actions workflow for automated releases
- Manual changelog update procedures
- Migration guide templates

**Bundled references:**

| File | Description |
|------|-------------|
| `references/semantic-release-config.md` | semantic-release setup |
| `references/conventional-commits-mapping.md` | Commit type to section mapping |

**Bundled examples:**

| File | Description |
|------|-------------|
| `examples/sample-changelog.md` | Example changelog |

## diataxis

Provides the Diataxis documentation framework for classifying, reviewing, and organizing documentation.

**Version:** 0.1.0
**Diataxis type:** reference

**Trigger phrases:** "apply diataxis", "classify documentation", "review docs with diataxis", "create diataxis documentation", "documentation gap analysis", "check documentation quadrants", "organize docs by type"

**Coverage:**

- The Diataxis compass: two axes (action/cognition, acquisition/application)
- Four documentation modes with structure templates and writing rules:
  - **Tutorial** -- learning-oriented, guided experience
  - **How-to** -- task-oriented, goal-focused steps
  - **Reference** -- information-oriented, technical descriptions
  - **Explanation** -- understanding-oriented, context and rationale
- Classification workflow
- Review with quadrant-specific quality criteria
- Gap analysis with coverage matrix
- Cross-reference audit
- Strictness levels: `advisory`, `standard`, `strict`

**Bundled references:**

| File | Description |
|------|-------------|
| `references/compass.md` | The Diataxis compass model |
| `references/quality-criteria.md` | Per-quadrant quality criteria |

**Bundled examples:**

| File | Description |
|------|-------------|
| `examples/tutorial-example.md` | Tutorial quadrant example |
| `examples/howto-example.md` | How-to quadrant example |
| `examples/reference-example.md` | Reference quadrant example |
| `examples/explanation-example.md` | Explanation quadrant example |

## See also

- [How to Review Documentation](../guides/review-documentation.md) — using the documentation-standards skill in practice
- [How to Create Documentation](../guides/create-documentation.md) — using the api-documentation skill in practice
- [How to Maintain a Changelog](../guides/maintain-changelog.md) — using the changelog skill in practice
- [How to Enforce Diátaxis](../guides/enforce-diataxis.md) — using the diataxis skill in practice
