---
description: Update outdated documentation with current information
argument-hint: "[path]"
allowed-tools: Read, Write, Edit, Glob, Grep, Bash(git:*)
---

Update documentation to reflect current codebase state.

## Target Scope

$IF($1,
  Update: @$1,
  Identify all outdated documentation in project
)

## Update Process

### 1. Detect Outdated Content

**Code-Documentation Drift**
- Compare documented APIs with actual implementation
- Check documented configuration options against code
- Verify documented dependencies against package files
- Compare documented CLI options with actual help output

**Version Drift**
- Find version numbers in documentation
- Compare against current releases
- Identify deprecated features still documented
- Find new features not yet documented

**Link Rot**
- Check internal links resolve to existing files
- Validate anchor links
- Test external links for accessibility

### 2. Gather Current State

For code drift:
```
# Get actual function signatures
# Get current configuration schema
# Get current CLI help
```

Use Grep and Read to:
- Extract current function signatures
- Read configuration schemas
- Parse CLI help output
- Find actual API endpoints

### 3. Update Categories

**API Changes**
- Update function signatures
- Add new parameters
- Remove deprecated options
- Update return types

**Configuration Changes**
- Add new config options
- Update default values
- Mark deprecated options
- Update environment variables

**Dependency Changes**
- Update version requirements
- Add new dependencies
- Remove deprecated ones
- Update installation commands

**Feature Changes**
- Document new features
- Update changed behaviors
- Mark deprecated features
- Add migration notes

### 4. Apply Updates

Use Edit tool to:
- Update specific sections
- Preserve document structure
- Maintain formatting consistency
- Add change markers if requested

### 5. Validation

After updates:
- Verify code examples still work
- Check all internal links
- Validate heading structure
- Run documentation review

## Output Format

```markdown
## Documentation Update Report

**Target:** [file or scope]
**Changes Made:** [count]

### Updates Applied

#### [filename]

| Section | Change Type | Description |
|---------|------------|-------------|
| API Reference | Updated | Function signature changed |
| Configuration | Added | New option: feature_flag |
| Dependencies | Updated | Version bumped to 2.0 |

### Details

**[filename]:[section]**
- Before: [old content snippet]
- After: [new content snippet]
- Reason: [why changed]

### Validation Results
- [ ] Code examples verified
- [ ] Links validated
- [ ] Structure maintained

### Recommended Review
Changes should be reviewed for:
1. [specific concern]
2. [additional check]
```

## Safeguards

- Always show diff before applying changes
- Preserve user customizations
- Don't remove content without confirmation
- Create backup suggestions for major changes
