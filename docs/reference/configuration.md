---
title: "Configuration"
diataxis_type: reference
---

# Configuration

All plugin configuration lives in `.claude/documentation-review.local.md`. The file uses YAML frontmatter for settings and a markdown body for project-specific notes.

Run `/doc-setup` to generate this file interactively. For a setup walkthrough, see the [Configure Project guide](../guides/configure-project.md).

## File location

```
.claude/documentation-review.local.md
```

This file is project-local and should be added to `.gitignore` or committed depending on team preference.

## Schema

### doc_paths

Paths to scan for documentation.

| Property | Value |
|----------|-------|
| Type | `array of strings` |
| Default | auto-detect |

```yaml
doc_paths:
  - docs/
  - README.md
  - "*.md"
```

### ignore

Glob patterns to exclude from scanning.

| Property | Value |
|----------|-------|
| Type | `array of strings` |
| Default | `node_modules`, `vendor`, `.git`, `dist`, `build`, `CHANGELOG.md` |

```yaml
ignore:
  - "**/node_modules/**"
  - "**/vendor/**"
  - "**/.git/**"
  - "**/dist/**"
  - "**/build/**"
  - "CHANGELOG.md"
```

### standards

Documentation quality standards.

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| `require_description` | boolean | `true` | First paragraph should describe purpose |
| `max_heading_depth` | integer | `4` | Maximum heading depth (avoids h5, h6) |
| `require_code_examples` | boolean | `false` | Whether code examples are required |
| `check_links` | boolean | `true` | Validate internal links |
| `check_spelling` | boolean | `false` | Spell checking (disabled by default, can be slow) |

```yaml
standards:
  require_description: true
  max_heading_depth: 4
  require_code_examples: false
  check_links: true
  check_spelling: false
```

### api_docs

API documentation settings.

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| `openapi_path` | string or null | `null` | Path to OpenAPI spec (auto-detect if null) |
| `asyncapi_path` | string or null | `null` | Path to AsyncAPI spec (auto-detect if null) |
| `generate_from_code` | boolean | `false` | Generate API docs from source code |

```yaml
api_docs:
  openapi_path: null
  asyncapi_path: null
  generate_from_code: false
```

#### Language doc toolchains

Each toolchain is configured under `api_docs.<toolchain>`. All toolchains share `enabled` and `build_command` keys, plus toolchain-specific options.

| Toolchain | Language | Build command example |
|-----------|----------|---------------------|
| `rustdoc` | Rust | `cargo doc --no-deps --all-features` |
| `godoc` | Go | -- (uses `packages` list) |
| `sphinx` | Python | `sphinx-build -b html docs/ docs/_build/` |
| `typedoc` | TypeScript | `npx typedoc` |
| `javadoc` | Java | `./gradlew javadoc` |
| `dokka` | Kotlin | `./gradlew dokkaHtml` |
| `docc` | Swift | `swift package generate-documentation` |
| `docfx` | C# | `docfx build` |
| `exdoc` | Elixir | `mix docs` |

Toolchain-specific options:

| Toolchain | Option | Type | Description |
|-----------|--------|------|-------------|
| `rustdoc` | `crates` | array | Specific crates to document (empty for all) |
| `rustdoc` | `warn_missing_docs` | boolean | Warn on missing documentation |
| `godoc` | `packages` | array | Package patterns to document |
| `sphinx` | `docstring_style` | string | `google`, `numpy`, or `rst` |
| `typedoc` | `entry_points` | array | TypeScript entry point files |
| `docc` | `target` | string or null | Specific target (null for all) |
| `docfx` | `config_path` | string | Path to `docfx.json` |

```yaml
api_docs:
  rustdoc:
    enabled: true
    build_command: "cargo doc --no-deps --all-features"
    crates: []
    warn_missing_docs: true
```

### site_generator

Static site generator integration.

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| `type` | string | `"auto"` | `auto`, `mkdocs`, `sphinx`, `docusaurus`, `astro` |
| `config_path` | string or null | `null` | Path to site generator config (auto-detect if null) |

```yaml
site_generator:
  type: auto
  config_path: null
```

### diataxis

Diataxis framework settings.

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| `enabled` | boolean | `true` | Enable Diataxis framework analysis |
| `strictness` | string | `"strict"` | `advisory`, `standard`, or `strict` |
| `require_frontmatter` | boolean | `true` | Require `diataxis_type` in document frontmatter |
| `doc_paths` | array | `[]` | Paths for Diataxis scanning (empty = use top-level `doc_paths`) |
| `quadrant_directories` | object | all null | Map quadrants to directories |

**Strictness levels:**

| Level | Behavior |
|-------|----------|
| `advisory` | Report Diataxis findings as suggestions only |
| `standard` | Flag mode mixing and missing classifications as issues |
| `strict` | Enforce quadrant purity; treat violations as errors |

**Quadrant directories:**

| Key | Description |
|-----|-------------|
| `tutorials` | Directory for tutorial documents |
| `how-to` | Directory for how-to guides |
| `reference` | Directory for reference documents |
| `explanation` | Directory for explanation documents |

```yaml
diataxis:
  enabled: true
  strictness: strict
  require_frontmatter: true
  doc_paths: []
  quadrant_directories:
    tutorials: docs/tutorials/
    how-to: docs/guides/
    reference: docs/reference/
    explanation: docs/explanation/
```

### output

Output format preferences.

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| `verbosity` | string | `"normal"` | `minimal`, `normal`, `detailed` |
| `format` | string | `"markdown"` | `markdown`, `json` |

```yaml
output:
  verbosity: normal
  format: markdown
```

## Full example

```yaml
---
doc_paths:
  - docs/
  - README.md

ignore:
  - "**/node_modules/**"
  - "**/dist/**"
  - "CHANGELOG.md"

standards:
  require_description: true
  max_heading_depth: 4
  require_code_examples: false
  check_links: true
  check_spelling: false

api_docs:
  openapi_path: null
  asyncapi_path: null
  generate_from_code: false

site_generator:
  type: auto
  config_path: null

diataxis:
  enabled: true
  strictness: strict
  require_frontmatter: true
  doc_paths: []
  quadrant_directories:
    tutorials: null
    how-to: null
    reference: null
    explanation: null

output:
  verbosity: normal
  format: markdown
---

# Project Documentation Notes

Add project-specific context here.
```

## Markdown body

The section below the YAML frontmatter accepts free-form markdown. Use it for:

- Project-specific documentation conventions
- Special terminology or naming patterns
- Style guide references
- Areas needing documentation attention
- Known documentation gaps

This content is available to all commands and agents as additional context.

## See also

- [How to Configure Your Project](../guides/configure-project.md) — step-by-step setup guide
- [Commands](commands.md) — commands that consume this configuration
- [Skills](skills.md) — skills referenced by configuration keys
- [Agents](agents.md) — agents that load configuration
- [Getting Started](../tutorials/getting-started.md) — first encounter with configuration
