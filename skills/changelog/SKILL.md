---
name: changelog
description: >-
  Generate, update, review, and maintain changelogs using Keep a Changelog format,
  semantic-release, and conventional commits. Use this skill whenever the user wants to
  add a changelog entry, prepare release notes, document version changes, generate a
  changelog from git history, configure semantic-release, understand conventional commit
  mappings, review changelog format, create migration guides for breaking changes, or
  troubleshoot semantic-release issues. Also applies when discussing version bumps,
  release automation, CHANGELOG.md formatting, or any task involving documenting what
  changed between software versions. If the user mentions "changelog", "release notes",
  "version history", "what changed", "semantic-release", or "conventional commits",
  this skill likely applies. Also covers "what's new", "upgrade guide", and
  "release history" queries.
---

# Changelog Management

Changelogs are the primary way users understand what changed between versions. A good changelog builds trust, reduces support tickets, and makes upgrade decisions easier. This skill covers the full lifecycle: format, generation, automation, and maintenance.

## Critical: Comparison Links

Every version entry in a Keep a Changelog file requires a comparison link at the bottom of the file. This is the most commonly forgotten element and breaks the linkability that makes changelogs useful. Always include them:

```markdown
[Unreleased]: https://github.com/org/repo/compare/v1.2.0...HEAD
[1.2.0]: https://github.com/org/repo/compare/v1.1.0...v1.2.0
[1.1.0]: https://github.com/org/repo/releases/tag/v1.1.0
```

When adding a new version entry, update both the new version's link AND the `[Unreleased]` link to point to the new tag.

## Changelog Overview

The recommended approach combines **Keep a Changelog** format with **semantic-release** for automation. This pairing works because Keep a Changelog is human-readable and widely recognized, while semantic-release eliminates manual version management:

- **Automatic Generation**: Changelog generated from git commits via conventional commit messages
- **Version Bumping**: Automatic semver versioning based on commit types (feat → minor, fix → patch)
- **Release Notes**: GitHub releases populated automatically
- **Breaking Changes**: Highlighted prominently with migration guidance
- **Categorized Changes**: Features, fixes, and other changes grouped logically

## Changelog Format

### Keep a Changelog Format

The project follows [Keep a Changelog](https://keepachangelog.com/) format:

```markdown
# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added
- New feature description

### Changed
- Change description

### Deprecated
- Deprecated feature notice

### Removed
- Removed feature notice

### Fixed
- Bug fix description

### Security
- Security fix description

## [1.2.0] - 2024-01-15

### Added
- Add blog post generation with Gemini AI
- Add social media integration (Discord, LinkedIn, Telegram, Twitter)

### Changed
- Update Next.js to v16
- Migrate to Cache Components mode

### Fixed
- Fix database connection timeout issue
- Fix chart rendering on mobile devices

## [1.1.0] - 2024-01-01

### Added
- Add COE bidding results endpoint
- Add interactive charts with Recharts

### Fixed
- Fix pagination in car makes endpoint

## [1.0.0] - 2023-12-15

### Added
- Initial release
- Car registration data API
- Next.js web application
- PostgreSQL database with Drizzle ORM
- Redis caching
- SST v3 infrastructure

[Unreleased]: https://github.com/username/repo/compare/v1.2.0...HEAD
[1.2.0]: https://github.com/username/repo/compare/v1.1.0...v1.2.0
[1.1.0]: https://github.com/username/repo/compare/v1.0.0...v1.1.0
[1.0.0]: https://github.com/username/repo/releases/tag/v1.0.0
```

## Semantic Release Configuration

### .releaserc.json

The release rules below separate user-visible changes (feat, fix, perf, revert → trigger releases) from internal changes (docs, style, chore, refactor, test, build, ci → no release). This prevents version noise from changes that don't affect consumers.

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
  "preset": "conventionalcommits",
  "releaseRules": [
    { "type": "feat", "release": "minor" },
    { "type": "fix", "release": "patch" },
    { "type": "perf", "release": "patch" },
    { "type": "revert", "release": "patch" },
    { "type": "docs", "release": false },
    { "type": "style", "release": false },
    { "type": "chore", "release": false },
    { "type": "refactor", "release": false },
    { "type": "test", "release": false },
    { "type": "build", "release": false },
    { "type": "ci", "release": false }
  ],
  "parserOpts": {
    "noteKeywords": ["BREAKING CHANGE", "BREAKING CHANGES"]
  }
}
```

## Automated Changelog Generation

### GitHub Actions

```yaml
# .github/workflows/release.yml
name: Release

on:
  push:
    branches:
      - main

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
          persist-credentials: false

      - uses: pnpm/action-setup@v2
      - uses: actions/setup-node@v4
        with:
          node-version: 20
          cache: "pnpm"

      - run: pnpm install

      - name: Release
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          NPM_TOKEN: ${{ secrets.NPM_TOKEN }}
        run: npx semantic-release
```

## Manual Changelog Updates

### Adding Unreleased Changes

Add entries under the `[Unreleased]` section as work is merged. This keeps the changelog current without waiting for release time.

### Creating a New Release Entry

```markdown
## [1.3.0] - 2024-02-01

### Added
- Add user authentication
- Add admin panel

### Changed
- Upgrade Next.js to v16.1.0
- Update database schema

### Fixed
- Fix memory leak in cache
- Fix broken links in documentation

### Security
- Update vulnerable dependencies
```

## Commit Types and Changelog Sections

| Commit Type | Changelog Section | Version Bump |
|-------------|------------------|--------------|
| `feat:` | Added | Minor (0.x.0) |
| `fix:` | Fixed | Patch (0.0.x) |
| `perf:` | Performance | Patch |
| `refactor:` | Changed | None |
| `docs:` | Documentation | None |
| `style:` | Changed | None |
| `test:` | None | None |
| `chore:` | None | None |
| `build:` | None | None |
| `ci:` | None | None |
| `revert:` | Fixed | Patch |
| `BREAKING CHANGE` | Breaking Changes | Major (x.0.0) |

## Example Changelog Entries

### Feature Addition

```markdown
## [1.2.0] - 2024-01-15

### Added
- **Blog Generation**: Automatically generate blog posts from car registration data using Google Gemini AI
  - Configurable prompts for different content styles
  - Automatic SEO optimization
  - Support for custom topics
- **Social Media Integration**: Post updates to Discord, LinkedIn, Telegram, and Twitter
  - Automated posting when new data is available
  - Customizable message templates
  - Rate limiting support
```

### Bug Fix

```markdown
### Fixed
- **Database**: Fix connection timeout issue when processing large datasets
  - Increased connection pool size
  - Added connection retry logic
  - Improved error handling
- **Charts**: Fix chart rendering on mobile devices
  - Updated responsive breakpoints
  - Fixed touch event handling
  - Improved performance on low-end devices
```

### Breaking Change

```markdown
## [2.0.0] - 2024-03-01

### BREAKING CHANGES

- **API**: Changed authentication method from API keys to JWT tokens
  - Migration guide: https://docs.sgcarstrends.com/migration/v2
  - API keys deprecated and will be removed in v2.1.0
  - Update clients to use new authentication flow

- **Database**: Renamed `cars` table to `vehicle_registrations`
  - Run migration: `pnpm db:migrate`
  - Update queries to use new table name
  - Backward compatibility maintained until v2.1.0

### Added
- New authentication system with JWT tokens
- Support for refresh tokens
```

## Changelog Best Practices

The goal is entries that help users decide whether to upgrade and what to watch out for. Vague entries ("fixed bugs") are worse than no entries — they signal carelessness.

### 1. Clear Descriptions
Write for users, not developers. Include what changed and why it matters:
- Good: "Add rate limiting to API endpoints (100 req/min per IP)"
- Bad: "Added stuff"

### 2. Group Related Changes
Keep all features under Added, all fixes under Fixed. Don't interleave types.

### 3. Include Context
For significant changes, add sub-bullets with impact, root cause, or performance data. Users making upgrade decisions need this context.

### 4. Link to Issues/PRs
Reference the PR or issue number for traceability: `([#123](url))`

## Viewing and Validating

### Quick Commands
```bash
# Preview without releasing
npx semantic-release --dry-run

# Lint CHANGELOG.md format
pnpm markdownlint CHANGELOG.md
```

For detailed generation commands, validation scripts, and local viewing tips, see `references/semantic-release-config.md`.

## Migration Guide Template

When creating breaking changes, include a migration guide:

```markdown
## [2.0.0] - 2024-03-01

### BREAKING CHANGES

#### Authentication Method Changed

**What changed:**
- API authentication now uses JWT tokens instead of API keys

**Migration steps:**

1. **Update client code** to use new authentication:
   ```javascript
   // Before
   headers: { 'X-API-Key': 'your-key' }

   // After
   headers: { 'Authorization': 'Bearer your-jwt-token' }
   \```

2. **Obtain JWT token** from new `/auth/login` endpoint:
   ```bash
   curl -X POST https://api.sgcarstrends.com/auth/login \
     -H 'Content-Type: application/json' \
     -d '{"username":"user","password":"pass"}'
   \```

3. **Update environment variables**:
   ```bash
   # Remove
   API_KEY=your-old-key

   # Add
   JWT_SECRET=your-jwt-secret
   \```

**Timeline:**
- API keys supported until: **April 1, 2024**
- Must migrate by: **March 31, 2024**

**Support:**
- Questions: [GitHub Discussions](https://github.com/username/repo/discussions)
- Issues: [GitHub Issues](https://github.com/username/repo/issues)
```

## Troubleshooting

### Semantic Release Not Generating Changelog
Common causes: no conventional commits since last release, wrong branch, or missing GITHUB_TOKEN. Check `git log --oneline` to verify commit format and `git branch` to verify you're on the release branch.

### Duplicate Entries
Clean up manually, then commit: `git commit -m "docs: remove duplicate changelog entries"`

## Additional Resources

### Reference Files

For detailed guidance, consult:
- **`references/semantic-release-config.md`** - Advanced semantic-release configuration patterns
- **`references/conventional-commits-mapping.md`** - Mapping from commit types to changelog sections

### Example Files

Working examples in `examples/`:
- **`sample-changelog.md`** - Complete changelog following Keep a Changelog format

### External References

- Keep a Changelog: https://keepachangelog.com
- Semantic Versioning: https://semver.org
- Semantic Release: https://semantic-release.gitbook.io
- Conventional Commits: https://www.conventionalcommits.org
- Related files:
  - `CHANGELOG.md` - Project changelog
  - `.releaserc.json` - Semantic release config
  - Root CLAUDE.md - Release process

## Summary

Automate generation with semantic-release, write clear user-focused entries, group by change type, highlight breaking changes prominently with migration guides, and link to issues/PRs for traceability.
