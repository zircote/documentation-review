# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added

- Diátaxis documentation framework skill with full quadrant specification (tutorial, how-to, reference, explanation)
- `/doc-diataxis` command with classify, review, gaps, and audit subcommands
- Language doc toolchain coverage: Rust, Go, Python, TypeScript, Java, Kotlin, Swift, C#, Elixir
- Astro Starlight support in site generators
- Diátaxis configuration options in config template
- `diataxis_type` frontmatter field for commands and skills

### Changed

- Diátaxis enabled by default at strict strictness level
- Renamed diataxis command to `/doc-diataxis` for naming consistency
- Commands and agents augmented with Diátaxis awareness

### Removed

- Standalone `/changelog` command (covered by changelog skill)
- Unused PostToolUse hooks from agents

## [0.1.0] - 2025-01-16

### Added

- Initial release as standalone plugin
- **Commands**:
  - `/doc-review` - Review documentation for quality issues
  - `/doc-create` - Generate new documentation from codebase
  - `/doc-update` - Update outdated documentation
  - `/doc-cleanup` - Identify obsolete documentation
  - `/doc-setup` - Interactive configuration wizard
  - `/changelog` - CHANGELOG.md management
- **Agents**:
  - `doc-reviewer` - Comprehensive documentation audit
  - `doc-writer` - Content generation and updates
- **Skills**:
  - `documentation-standards` - Markdown best practices and quality guidelines
  - `api-documentation` - OpenAPI/Swagger/AsyncAPI patterns
  - `changelog` - Keep a Changelog format and semantic-release integration
- Configuration via `.claude/documentation-review.local.md`
- Support for MkDocs, Sphinx, and Docusaurus awareness
- GitHub Actions CI workflow for plugin validation

[Unreleased]: https://github.com/zircote/documentation-review/compare/v0.1.0...HEAD
[0.1.0]: https://github.com/zircote/documentation-review/releases/tag/v0.1.0
