---
title: "How to Configure Documentation Review for Your Project"
diataxis_type: how-to
---

# How to Configure Documentation Review for Your Project

This guide explains how to set up and customize the documentation-review plugin configuration for your project.

## Prerequisites

- The documentation-review plugin installed (`claude plugin install zircote/documentation-review`)

## Use Interactive Setup

If you want guided configuration, run:

```
/doc-setup
```

The setup wizard detects your project structure (documentation paths, ignore patterns, static site generators, API specs) and walks you through each setting. It writes the configuration file for you.

If you already have a configuration file and want to update it, run:

```
/doc-setup --update
```

## Create Configuration Manually

If you prefer to create the configuration by hand, create the file `.claude/documentation-review.local.md` with YAML frontmatter.

### Minimal configuration

```yaml
---
doc_paths:
  - docs/
  - README.md

ignore:
  - "**/node_modules/**"
  - "**/dist/**"
---
```

### Full configuration

```yaml
---
doc_paths:
  - docs/
  - README.md
  - "*.md"

ignore:
  - "**/node_modules/**"
  - "**/vendor/**"
  - "**/.git/**"
  - "**/dist/**"
  - "**/build/**"
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

Add project-specific context here for the review agents.
```

## Customize Documentation Paths

If you want to control which files are reviewed, set `doc_paths` to the directories and patterns that contain your documentation:

```yaml
doc_paths:
  - docs/
  - guides/
  - README.md
  - CONTRIBUTING.md
```

If you want to exclude specific files or directories, add them to `ignore`:

```yaml
ignore:
  - "**/node_modules/**"
  - "**/generated/**"
  - "docs/legacy/**"
```

## Configure Quality Standards

Adjust the `standards` section to match your project's documentation requirements:

| Setting | Default | Purpose |
|---------|---------|---------|
| `require_description` | `true` | First paragraph must describe the document's purpose |
| `max_heading_depth` | `4` | Flag headings deeper than this level |
| `require_code_examples` | `false` | Require code examples in documentation |
| `check_links` | `true` | Validate internal links |
| `check_spelling` | `false` | Run spell checking (can be slow) |

## Set Up API Documentation

### Point to an existing spec

If your project has an OpenAPI or AsyncAPI specification, set the path:

```yaml
api_docs:
  openapi_path: api/openapi.yaml
```

### Enable code-based generation

If you want to generate API docs from source code instead of a spec file:

```yaml
api_docs:
  generate_from_code: true
```

### Configure a language doc toolchain

If your project uses a language-specific documentation tool, enable it under `api_docs`. For example, for Rust:

```yaml
api_docs:
  rustdoc:
    enabled: true
    build_command: "cargo doc --no-deps --all-features"
    crates: []
    warn_missing_docs: true
```

Supported toolchains: `rustdoc`, `godoc`, `sphinx`, `typedoc`, `javadoc`, `dokka`, `docc`, `docfx`, `exdoc`.

## Configure Site Generator Integration

If your project uses a static site generator, the plugin can integrate with it. Set the type explicitly or leave it on `auto` for detection:

```yaml
site_generator:
  type: mkdocs           # mkdocs, sphinx, docusaurus, astro, or auto
  config_path: mkdocs.yml
```

## Configure Diataxis Settings

Diataxis analysis is enabled by default. If you want to adjust it:

### Change strictness level

```yaml
diataxis:
  strictness: advisory    # advisory, standard, or strict
```

- **advisory** -- suggestions only, no errors
- **standard** -- flags issues in the review report
- **strict** -- treats violations as errors

### Organize docs by quadrant directory

If your documentation is organized into directories by Diataxis quadrant:

```yaml
diataxis:
  quadrant_directories:
    tutorials: docs/tutorials/
    how-to: docs/guides/
    reference: docs/reference/
    explanation: docs/explanation/
```

### Disable Diataxis

If you do not want Diataxis analysis:

```yaml
diataxis:
  enabled: false
```

## Verify Your Configuration

After creating or updating the configuration, run a review to confirm it works as expected:

```
/doc-review
```

Check that the correct files are scanned, the right standards are applied, and the output matches your expectations.

## Related

- [Configuration reference](../reference/configuration.md) -- complete reference for all configuration keys
- [Getting started tutorial](../tutorials/getting-started.md) -- first-time setup walkthrough
- [Enforce Diataxis](enforce-diataxis.md) -- detailed guide for Diataxis configuration
