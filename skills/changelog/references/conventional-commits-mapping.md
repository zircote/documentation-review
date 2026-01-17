# Conventional Commits to Changelog Mapping

Reference for mapping conventional commit types to Keep a Changelog sections.

## Standard Mapping

| Commit Type | Changelog Section | Version Bump | Example |
|-------------|-------------------|--------------|---------|
| `feat:` | Added | Minor (0.x.0) | `feat: add user authentication` |
| `fix:` | Fixed | Patch (0.0.x) | `fix: resolve login timeout` |
| `perf:` | Performance | Patch | `perf: optimize database queries` |
| `revert:` | Reverted | Patch | `revert: remove broken feature` |
| `docs:` | Documentation | None* | `docs: update API reference` |
| `style:` | (hidden) | None | `style: format code` |
| `refactor:` | Changed | None* | `refactor: restructure auth module` |
| `test:` | (hidden) | None | `test: add unit tests` |
| `build:` | (hidden) | None | `build: update dependencies` |
| `ci:` | (hidden) | None | `ci: add GitHub Actions` |
| `chore:` | (hidden) | None | `chore: update gitignore` |
| `BREAKING CHANGE` | Breaking Changes | Major (x.0.0) | `feat!: change API interface` |

*Can be configured to trigger releases in certain cases.

## Commit Message Format

```
<type>(<scope>): <description>

[optional body]

[optional footer(s)]
```

### Examples

**Simple feature:**
```
feat: add password reset functionality
```

**Feature with scope:**
```
feat(auth): add OAuth2 support for Google
```

**Fix with body:**
```
fix(api): resolve rate limiting issue

The rate limiter was not properly resetting counters
after the time window expired. This caused legitimate
requests to be blocked.

Closes #123
```

**Breaking change:**
```
feat(api)!: change authentication to JWT

BREAKING CHANGE: API authentication now requires JWT tokens
instead of API keys. See migration guide for details.

Migration steps:
1. Generate JWT token from /auth/login
2. Include token in Authorization header
3. Update client libraries to v2.0
```

## Keep a Changelog Sections

### Added
For new features.

**From commits:**
- `feat: ...`
- `feat(scope): ...`

**Example entries:**
```markdown
### Added
- **Authentication**: Add OAuth2 support for Google and GitHub
  - Supports authorization code flow
  - Automatic token refresh
  - Session management included
- **API**: Add bulk import endpoint for user data
```

### Changed
For changes in existing functionality.

**From commits:**
- `refactor: ...` (when visible to users)
- `style: ...` (when affecting output)

**Example entries:**
```markdown
### Changed
- **Dashboard**: Redesign metrics display for better readability
- **API**: Update response format to include pagination metadata
```

### Deprecated
For soon-to-be removed features.

**Note:** Usually added manually, not from commit type.

**Example entries:**
```markdown
### Deprecated
- **API**: The `/v1/users` endpoint is deprecated, use `/v2/users` instead
  - Will be removed in v3.0.0
  - Migration guide: [link]
```

### Removed
For now removed features.

**Note:** Usually added manually or from BREAKING CHANGE.

**Example entries:**
```markdown
### Removed
- **CLI**: Remove `--legacy` flag (deprecated since v1.5.0)
- **API**: Remove XML response format
```

### Fixed
For bug fixes.

**From commits:**
- `fix: ...`
- `fix(scope): ...`

**Example entries:**
```markdown
### Fixed
- **Database**: Fix connection pool exhaustion under high load
  - Properly close idle connections
  - Add connection timeout handling
- **UI**: Fix chart rendering on Safari mobile
```

### Security
For security fixes.

**From commits:**
- `fix(security): ...`
- Usually flagged manually for visibility

**Example entries:**
```markdown
### Security
- **Auth**: Fix session fixation vulnerability ([CVE-2024-1234](link))
- **API**: Add rate limiting to prevent brute force attacks
```

## Scopes

Common scopes help organize changelog entries:

| Scope | Description |
|-------|-------------|
| `api` | API changes |
| `auth` | Authentication/authorization |
| `cli` | Command-line interface |
| `config` | Configuration |
| `core` | Core functionality |
| `db` | Database |
| `deps` | Dependencies |
| `docs` | Documentation |
| `security` | Security-related |
| `ui` | User interface |

## Breaking Changes

### Indicators

Any of these mark a breaking change:
- `!` after type: `feat!: ...`
- `BREAKING CHANGE:` in footer
- `BREAKING-CHANGE:` in footer
- `BREAKING CHANGES:` in footer

### Example Formats

**With exclamation:**
```
feat(api)!: require authentication for all endpoints
```

**With footer:**
```
feat(api): change response format

BREAKING CHANGE: Response now wraps data in `{ data: ... }`
instead of returning raw data.
```

### Changelog Entry

```markdown
## [2.0.0] - 2024-03-01

### BREAKING CHANGES

- **API**: Authentication now required for all endpoints
  - Unauthenticated requests return 401
  - Use `/auth/token` to obtain access token
  - See [Migration Guide](link)

- **Response Format**: All responses now wrapped in data object
  ```json
  // Before
  { "id": 1, "name": "User" }

  // After
  { "data": { "id": 1, "name": "User" } }
  ```
```

## Configuration Examples

### semantic-release

```json
{
  "releaseRules": [
    { "type": "feat", "release": "minor" },
    { "type": "fix", "release": "patch" },
    { "type": "perf", "release": "patch" },
    { "type": "revert", "release": "patch" },
    { "breaking": true, "release": "major" }
  ],
  "parserOpts": {
    "noteKeywords": ["BREAKING CHANGE", "BREAKING CHANGES"]
  }
}
```

### release-notes-generator

```json
{
  "presetConfig": {
    "types": [
      { "type": "feat", "section": "Features" },
      { "type": "fix", "section": "Bug Fixes" },
      { "type": "perf", "section": "Performance" },
      { "type": "revert", "section": "Reverts" },
      { "type": "docs", "section": "Documentation", "hidden": false },
      { "type": "refactor", "section": "Refactoring", "hidden": true }
    ]
  }
}
```

## Best Practices

### Writing Commit Messages

1. **Use imperative mood**: "add feature" not "added feature"
2. **Keep subject under 72 characters**
3. **Capitalize first letter after type**
4. **No period at end of subject**
5. **Include scope when helpful**
6. **Add body for complex changes**
7. **Reference issues in footer**

### Writing Changelog Entries

1. **User-focused language**: Describe impact, not implementation
2. **Group related changes**: Under component/feature headings
3. **Include context**: Why the change matters
4. **Link to issues/PRs**: For traceability
5. **Highlight breaking changes**: With migration steps

### Example Transformation

**Commit:**
```
fix(auth): handle expired JWT tokens gracefully

Previously, expired tokens caused 500 errors. Now returns
proper 401 with clear error message.

Closes #456
```

**Changelog entry:**
```markdown
### Fixed
- **Authentication**: Return proper 401 error for expired tokens
  instead of 500 server error ([#456](link))
```

## Related Resources

- [Conventional Commits Specification](https://www.conventionalcommits.org/)
- [Keep a Changelog](https://keepachangelog.com/)
- [Semantic Versioning](https://semver.org/)
