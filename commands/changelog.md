---
description: Manage CHANGELOG.md entries and release documentation
argument-hint: "<action: add|review|generate|preview>"
allowed-tools: Read, Write, Edit, Glob, Grep, Bash(git:*)
---

Manage changelog entries and release documentation.

## Action

$IF($1,
  Perform action: $1,
  Ask which changelog action to perform using AskUserQuestion:
  - add: Add new entry to [Unreleased] section
  - review: Review current changelog for issues
  - generate: Generate changelog from git commits
  - preview: Preview upcoming release notes
)

## Actions

### add - Add Changelog Entry

1. **Read current CHANGELOG.md** (create if missing)

2. **Gather information**
   - Use Bash tool to get recent commits: `git log --oneline -20`
   - Identify commit types (feat, fix, etc.)
   - Group by category

3. **Add to [Unreleased] section**
   Categories in order:
   - Added (feat commits)
   - Changed (refactor, style)
   - Deprecated
   - Removed
   - Fixed (fix commits)
   - Security

4. **Format entries**
   ```markdown
   ### Added
   - **[Component]**: Description of feature
     - Additional details if needed
     - Link to PR: ([#123](url))
   ```

### review - Review Changelog

1. **Check structure**
   - Verify Keep a Changelog format
   - Check version ordering (newest first)
   - Validate date formats (YYYY-MM-DD)
   - Verify comparison links at bottom

2. **Check content quality**
   - Clear, user-focused descriptions
   - Proper categorization
   - No developer jargon
   - Links to issues/PRs where relevant

3. **Cross-reference with code**
   - Compare documented changes with actual commits
   - Identify missing entries
   - Find outdated entries

4. **Report findings**
   - Format issues
   - Missing entries
   - Suggested improvements

### generate - Generate from Git

1. **Analyze git history**
   Get commits since last release using Bash tool:
   - First, get latest tag: `git describe --tags --abbrev=0 2>/dev/null || echo ""`
   - Then get commits since that tag (or last 50 if no tags): `git log <tag>..HEAD --pretty=format:"%h %s"` or `git log -50 --pretty=format:"%h %s"`

2. **Parse conventional commits**
   Map to changelog categories:
   - feat: → Added
   - fix: → Fixed
   - perf: → Performance
   - BREAKING CHANGE → Breaking Changes
   - docs: → Documentation
   - refactor: → Changed

3. **Generate changelog section**
   Create properly formatted entries for [Unreleased]

4. **Present for review**
   Show generated content before applying

### preview - Preview Release Notes

1. **Read [Unreleased] section**

2. **Format as release notes**
   ```markdown
   # Release v[next-version]

   ## Highlights
   [Key features/changes]

   ## What's Changed
   [Categorized changes]

   ## Breaking Changes
   [If any, with migration notes]

   ## Contributors
   [From git log]
   ```

3. **Suggest version number**
   Based on changes:
   - Breaking changes → Major bump
   - New features → Minor bump
   - Bug fixes only → Patch bump

## Output

Report action results:

```markdown
## Changelog Action: [action]

**File:** CHANGELOG.md
**Changes:** [description]

### Summary
[What was done]

### Next Steps
1. [Recommended action]
```

## Keep a Changelog Format Reference

```markdown
# Changelog

All notable changes to this project will be documented in this file.

## [Unreleased]

### Added
- New features

### Changed
- Changes in existing functionality

### Deprecated
- Soon-to-be removed features

### Removed
- Removed features

### Fixed
- Bug fixes

### Security
- Security fixes

## [1.0.0] - YYYY-MM-DD
...
```

Use the changelog skill for detailed formatting guidance.
