---
description: Identify and report obsolete or outdated documentation
allowed-tools: Read, Glob, Grep, Bash(git:*)
---

Identify obsolete documentation for user review. This command reports only - it does not delete files.

## Cleanup Analysis

### 1. Find Documentation Files

Use Glob to locate all documentation:
- `docs/**/*.md`
- `*.md` (root level)
- `**/*.rst` (if RST supported)

### 2. Detect Obsolete Content

**Orphaned Documentation**
- Files not linked from any other document
- Files not in navigation/sidebar config
- README files in deleted directories

**Stale Content Indicators**
- Last modified date > 1 year ago
- References to deleted files/directories
- Mentions deprecated APIs or features
- Version numbers significantly behind current

**Duplicate Content**
- Similar content across multiple files
- Copied sections that may be out of sync
- Redundant documentation of same feature

**Dead References**
- Internal links to non-existent files
- Anchor links to removed sections
- Image references to missing assets

### 3. Check Against Codebase

**Removed Features**
- Documentation for deleted code
- APIs that no longer exist
- Configuration options removed
- CLI commands deprecated

**Renamed Components**
- Old names still in documentation
- Outdated file paths
- Changed package/module names

### 4. Analyze Git History

Check for:
- Files unchanged for long periods
- Files with many authors (may indicate confusion)
- Files deleted and restored (uncertain status)

## Report Format

```markdown
# Documentation Cleanup Report

**Generated:** [date]
**Files Analyzed:** [count]
**Issues Found:** [count]

## Obsolete Documentation

### High Confidence (Safe to Remove)

| File | Reason | Last Modified |
|------|--------|---------------|
| docs/old-api.md | API deleted in v2.0 | 2023-01-15 |
| docs/legacy-setup.md | Feature deprecated | 2022-06-20 |

### Medium Confidence (Review Recommended)

| File | Reason | Notes |
|------|--------|-------|
| docs/advanced-config.md | No internal links | May be standalone guide |
| README-old.md | Possibly backup | Check if needed |

### Low Confidence (Needs Investigation)

| File | Observation |
|------|-------------|
| docs/troubleshooting.md | Not updated in 18 months |

## Duplicate Content

| Primary | Duplicate | Similarity |
|---------|-----------|------------|
| docs/setup.md | docs/getting-started.md | 85% |

## Broken References

| File | Broken Link | Type |
|------|-------------|------|
| README.md | ./docs/old-guide.md | Internal |
| docs/api.md | #deprecated-section | Anchor |

## Recommended Actions

1. **Remove** (high confidence): [list of files]
2. **Consolidate**: [duplicates to merge]
3. **Update**: [files with broken references]
4. **Investigate**: [uncertain files]

## Important Notes

- This report is advisory only
- No files have been modified
- Review each item before taking action
- Consider archiving instead of deleting
```

## User Guidance

After presenting the report:

1. Ask if user wants to act on any findings
2. For removals, confirm each file individually
3. For consolidation, ask which version to keep
4. For updates, offer to fix broken references

Never delete files automatically - always get explicit user confirmation.
