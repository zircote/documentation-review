# Semantic Release Configuration Patterns

Advanced configuration patterns for semantic-release in various project types.

## Basic Configuration

### Minimal `.releaserc.json`

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
  ]
}
```

### Extended Configuration

```json
{
  "branches": [
    "main",
    { "name": "next", "prerelease": true },
    { "name": "beta", "prerelease": true },
    { "name": "alpha", "prerelease": true }
  ],
  "plugins": [
    ["@semantic-release/commit-analyzer", {
      "preset": "conventionalcommits",
      "releaseRules": [
        { "type": "feat", "release": "minor" },
        { "type": "fix", "release": "patch" },
        { "type": "perf", "release": "patch" },
        { "type": "revert", "release": "patch" },
        { "type": "docs", "scope": "README", "release": "patch" },
        { "breaking": true, "release": "major" }
      ],
      "parserOpts": {
        "noteKeywords": ["BREAKING CHANGE", "BREAKING CHANGES", "BREAKING"]
      }
    }],
    ["@semantic-release/release-notes-generator", {
      "preset": "conventionalcommits",
      "presetConfig": {
        "types": [
          { "type": "feat", "section": "Features" },
          { "type": "fix", "section": "Bug Fixes" },
          { "type": "perf", "section": "Performance" },
          { "type": "revert", "section": "Reverts" },
          { "type": "docs", "section": "Documentation", "hidden": false },
          { "type": "style", "section": "Styles", "hidden": true },
          { "type": "chore", "section": "Miscellaneous", "hidden": true },
          { "type": "refactor", "section": "Code Refactoring", "hidden": true },
          { "type": "test", "section": "Tests", "hidden": true },
          { "type": "build", "section": "Build System", "hidden": true },
          { "type": "ci", "section": "CI", "hidden": true }
        ]
      }
    }],
    ["@semantic-release/changelog", {
      "changelogFile": "CHANGELOG.md",
      "changelogTitle": "# Changelog\n\nAll notable changes to this project will be documented in this file."
    }],
    ["@semantic-release/npm", {
      "npmPublish": true
    }],
    ["@semantic-release/github", {
      "assets": [
        { "path": "dist/**/*.js", "label": "Distribution" },
        { "path": "CHANGELOG.md", "label": "Changelog" }
      ],
      "successComment": "This ${issue.pull_request ? 'PR is included' : 'issue has been resolved'} in version ${nextRelease.version}",
      "failComment": false,
      "labels": ["released"]
    }],
    ["@semantic-release/git", {
      "assets": ["CHANGELOG.md", "package.json", "package-lock.json"],
      "message": "chore(release): ${nextRelease.version} [skip ci]\n\n${nextRelease.notes}"
    }]
  ]
}
```

## Project-Specific Configurations

### Monorepo with Lerna/Nx

```json
{
  "branches": ["main"],
  "extends": "semantic-release-monorepo",
  "plugins": [
    "@semantic-release/commit-analyzer",
    "@semantic-release/release-notes-generator",
    ["@semantic-release/changelog", {
      "changelogFile": "CHANGELOG.md"
    }],
    ["@semantic-release/npm", {
      "npmPublish": true
    }],
    ["@semantic-release/git", {
      "assets": ["CHANGELOG.md", "package.json"]
    }]
  ]
}
```

### Python Projects

```json
{
  "branches": ["main"],
  "plugins": [
    "@semantic-release/commit-analyzer",
    "@semantic-release/release-notes-generator",
    "@semantic-release/changelog",
    ["@semantic-release/exec", {
      "prepareCmd": "sed -i 's/^version = .*/version = \"${nextRelease.version}\"/' pyproject.toml",
      "publishCmd": "poetry publish --build"
    }],
    "@semantic-release/github",
    ["@semantic-release/git", {
      "assets": ["CHANGELOG.md", "pyproject.toml"]
    }]
  ]
}
```

### Go Projects

```json
{
  "branches": ["main"],
  "plugins": [
    "@semantic-release/commit-analyzer",
    "@semantic-release/release-notes-generator",
    "@semantic-release/changelog",
    ["@semantic-release/exec", {
      "prepareCmd": "echo 'v${nextRelease.version}' > VERSION"
    }],
    "@semantic-release/github",
    ["@semantic-release/git", {
      "assets": ["CHANGELOG.md", "VERSION"]
    }]
  ]
}
```

### No NPM Publish (Libraries/Apps)

```json
{
  "branches": ["main"],
  "plugins": [
    "@semantic-release/commit-analyzer",
    "@semantic-release/release-notes-generator",
    "@semantic-release/changelog",
    ["@semantic-release/npm", {
      "npmPublish": false
    }],
    "@semantic-release/github",
    ["@semantic-release/git", {
      "assets": ["CHANGELOG.md", "package.json"]
    }]
  ]
}
```

## Branch Strategies

### Multiple Release Channels

```json
{
  "branches": [
    "+([0-9])?(.{+([0-9]),x}).x",
    "main",
    { "name": "next", "prerelease": true },
    { "name": "next-major", "prerelease": true },
    { "name": "beta", "prerelease": true },
    { "name": "alpha", "prerelease": true }
  ]
}
```

### Maintenance Branches

```json
{
  "branches": [
    { "name": "1.x", "range": "1.x" },
    { "name": "2.x", "range": "2.x" },
    "main"
  ]
}
```

## Custom Release Rules

### Include Documentation Changes

```json
{
  "releaseRules": [
    { "type": "docs", "scope": "README", "release": "patch" },
    { "type": "docs", "scope": "api", "release": "patch" },
    { "type": "refactor", "scope": "core", "release": "patch" }
  ]
}
```

### Breaking Changes Detection

```json
{
  "releaseRules": [
    { "breaking": true, "release": "major" },
    { "type": "feat", "release": "minor" },
    { "type": "fix", "release": "patch" }
  ],
  "parserOpts": {
    "noteKeywords": [
      "BREAKING CHANGE",
      "BREAKING CHANGES",
      "BREAKING"
    ]
  }
}
```

## GitHub Actions Workflow

```yaml
name: Release

on:
  push:
    branches:
      - main
      - next
      - beta
      - "*.x"

permissions:
  contents: write
  issues: write
  pull-requests: write

jobs:
  release:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0
          persist-credentials: false

      - uses: actions/setup-node@v4
        with:
          node-version: 20
          cache: npm

      - run: npm ci

      - name: Release
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          NPM_TOKEN: ${{ secrets.NPM_TOKEN }}
        run: npx semantic-release
```

## Troubleshooting

### Common Issues

**No release created:**
- Verify commits follow conventional format
- Check branch configuration matches current branch
- Ensure GITHUB_TOKEN has correct permissions

**Changelog not updated:**
- Confirm `@semantic-release/changelog` is installed
- Check plugin order (changelog before git)
- Verify `changelogFile` path is correct

**Version not bumped in package.json:**
- Ensure `@semantic-release/npm` or `@semantic-release/git` includes package.json in assets
- Check npm plugin configuration

**GitHub release not created:**
- Verify `@semantic-release/github` is configured
- Check GITHUB_TOKEN permissions
- Ensure `contents: write` permission in workflow

### Dry Run Testing

```bash
# Preview what would happen
npx semantic-release --dry-run

# With debug output
DEBUG=semantic-release:* npx semantic-release --dry-run
```

## Related Resources

- [semantic-release documentation](https://semantic-release.gitbook.io/)
- [conventional-commits](https://www.conventionalcommits.org/)
- [keep-a-changelog](https://keepachangelog.com/)
