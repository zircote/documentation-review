---
description: Review documentation for quality, accuracy, and completeness
argument-hint: "[path]"
allowed-tools: Read, Glob, Grep, Bash(git:*), Bash(rg:*)
---

Review documentation for quality issues.

## Target Scope

$IF($1,
  Focus on: @$1,
  Scan all documentation in configured paths
)

## Pre-Review: Check Memory

Before reviewing, search mnemonic for relevant context:

```bash
# Check for prior review patterns and known issues
rg -i "documentation|review|quality" ~/.claude/mnemonic/*/patterns/ --glob "*.memory.md" -l | head -5

# Check for project-specific standards or exceptions
rg -i "standard|exception|ignore" ~/.claude/mnemonic/*/decisions/ --glob "*.memory.md" -l | head -5
```

Apply found context to inform the review process.

## Configuration Check

Use Glob tool to check if `.claude/documentation-review.local.md` exists.

If configuration exists, read it with Read tool for custom settings.
Otherwise, use default paths: `docs/`, `README.md`, `*.md`

## Review Process

1. **Inventory Documentation**
   - Use Glob to find all markdown files in scope
   - List files with their last modified dates

2. **Content Analysis**
   For each document, evaluate:

   **Accuracy**
   - Verify code examples compile/run
   - Check that API references match implementation
   - Validate version numbers are current
   - Confirm links are not broken

   **Completeness**
   - Ensure all sections have content
   - Check for missing prerequisites
   - Verify examples cover common use cases
   - Look for TODO or placeholder text

   **Clarity**
   - Assess heading structure (proper hierarchy)
   - Check for undefined technical terms
   - Evaluate sentence complexity
   - Look for ambiguous instructions

   **Formatting**
   - Verify code blocks have language tags
   - Check list formatting consistency
   - Validate table structure
   - Assess whitespace usage

3. **Generate Report**

   Organize findings by severity:

   ### Critical Issues
   - Incorrect technical information
   - Broken code examples
   - Missing critical sections

   ### Major Issues
   - Outdated content
   - Broken links
   - Unclear procedures

   ### Minor Issues
   - Formatting inconsistencies
   - Typos and grammar
   - Style variations

   ### Suggestions
   - Additional examples
   - Diagrams or visuals
   - Cross-references

## Output Format

```markdown
# Documentation Review Report

**Scope:** [files reviewed]
**Date:** [review date]

## Summary
- Total files: X
- Critical issues: X
- Major issues: X
- Minor issues: X

## Findings by File

### [filename]
**Score:** X/5

#### Issues
1. [Issue description]
   - Location: [line/section]
   - Severity: [critical/major/minor]
   - Recommendation: [how to fix]

## Prioritized Action Items
1. [Most important fix]
2. [Second priority]
...
```

Use the documentation-standards skill for detailed review criteria.
