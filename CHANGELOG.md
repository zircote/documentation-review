# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

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

[0.1.0]: https://github.com/zircote/documentation-review/releases/tag/v0.1.0
