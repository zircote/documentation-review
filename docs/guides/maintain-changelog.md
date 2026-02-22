---
title: "How to Maintain a Changelog"
diataxis_type: how-to
---

# How to Maintain a Changelog

This guide covers maintaining a changelog using Keep a Changelog format, conventional commits, and semantic versioning.

## Prerequisites

- A `CHANGELOG.md` file in your project root (or willingness to create one)
- Familiarity with [conventional commits](https://www.conventionalcommits.org/) syntax

## Use Keep a Changelog Format

If you are starting a new changelog, structure it like this:

```markdown
# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]
```

All new entries go under the `[Unreleased]` heading until you cut a release.

## Add Entries Under Unreleased

When you complete a change, add it to the appropriate section under `[Unreleased]`:

```markdown
## [Unreleased]

### Added
- Add user authentication with JWT tokens

### Fixed
- Fix database connection timeout during large imports
```

Use these section headings, in this order:

| Section | For |
|---------|-----|
| **Added** | New features |
| **Changed** | Changes to existing functionality |
| **Deprecated** | Features that will be removed |
| **Removed** | Features that were removed |
| **Fixed** | Bug fixes |
| **Security** | Security-related changes |

## Map Conventional Commits to Sections

If your project uses conventional commits, map commit types to changelog sections:

| Commit type | Changelog section | Version bump |
|-------------|------------------|--------------|
| `feat:` | Added | Minor |
| `fix:` | Fixed | Patch |
| `perf:` | Performance | Patch |
| `refactor:` | Changed | None |
| `docs:` | Documentation | None |
| `BREAKING CHANGE` | Breaking Changes | Major |

Not all commit types need changelog entries. Types like `chore:`, `test:`, `ci:`, and `build:` are typically omitted from the user-facing changelog.

## Create a Release Entry

When you are ready to release, move the `[Unreleased]` content into a new versioned section:

```markdown
## [Unreleased]

## [1.3.0] - 2026-02-22

### Added
- Add user authentication with JWT tokens
- Add admin panel for user management

### Fixed
- Fix database connection timeout during large imports
```

Add a comparison link at the bottom of the file:

```markdown
[Unreleased]: https://github.com/user/repo/compare/v1.3.0...HEAD
[1.3.0]: https://github.com/user/repo/compare/v1.2.0...v1.3.0
```

## Write Clear Entries

Each entry should describe what changed and why it matters to users:

```markdown
### Added
- Add rate limiting to API endpoints (100 requests/min per IP)

### Fixed
- Fix chart rendering on iOS Safari when viewport width is below 375px
  - Updated responsive breakpoints
  - Fixed touch event handling
```

If a change is breaking, document the migration path:

```markdown
### BREAKING CHANGES
- Change authentication from API keys to JWT tokens
  - Migration: update `Authorization` header from `X-API-Key: key` to `Bearer token`
  - API keys supported until v2.1.0
```

## Set Up Automated Changelog Generation

If you want to generate changelog entries automatically from conventional commits, configure semantic-release.

### Create `.releaserc.json`

```json
{
  "branches": ["main"],
  "plugins": [
    "@semantic-release/commit-analyzer",
    "@semantic-release/release-notes-generator",
    "@semantic-release/changelog",
    "@semantic-release/npm",
    "@semantic-release/github",
    "@semantic-release/git"
  ],
  "preset": "conventionalcommits"
}
```

### Add a GitHub Actions workflow

```yaml
# .github/workflows/release.yml
name: Release
on:
  push:
    branches: [main]

jobs:
  release:
    runs-on: ubuntu-latest
    permissions:
      contents: write
      issues: write
      pull-requests: write
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0
      - uses: actions/setup-node@v4
        with:
          node-version: 20
      - run: npm install
      - run: npx semantic-release
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

With this in place, merging conventional commits to `main` automatically updates the changelog and creates GitHub releases.

## Related

- [Skills reference](../reference/skills.md) -- changelog skill details and best practices
- [Configuration reference](../reference/configuration.md) -- plugin configuration options
- [Review documentation](review-documentation.md) -- validate your changelog with a review
