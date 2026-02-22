---
title: "How to Review Documentation for Quality Issues"
diataxis_type: how-to
---

# How to Review Documentation for Quality Issues

This guide walks you through running a documentation review, interpreting the results, and fixing the findings.

## Prerequisites

- The documentation-review plugin installed (`claude plugin install zircote/documentation-review`)
- A project with markdown documentation

## Run a Documentation Review

### Review the entire project

If you want to review all documentation in your project, run the command with no arguments:

```
/doc-review
```

The plugin scans all paths defined in your configuration (or the defaults: `docs/`, `README.md`, `*.md`).

### Review a specific path

If you want to review only a particular file or directory, pass the path as an argument:

```
/doc-review docs/api/
/doc-review README.md
```

## Interpret the Report

The review produces a structured report organized by severity. Each finding includes a location, severity level, and recommendation.

### Severity levels

| Severity | Meaning | Action |
|----------|---------|--------|
| **Critical** | Incorrect technical information, broken code examples, missing critical sections | Fix immediately |
| **Major** | Outdated content, broken links, unclear procedures | Fix before next release |
| **Minor** | Formatting inconsistencies, typos, style variations | Fix when convenient |
| **Suggestion** | Additional examples, diagrams, cross-references | Consider for improvement |

Each file also receives a score from 1 to 5, giving you a quick sense of overall quality.

### Diataxis alignment

If Diataxis is enabled (the default), the report also includes:

- The detected quadrant for each document (tutorial, how-to, reference, explanation)
- Mode purity scores flagging sections where content drifts into another quadrant
- Missing frontmatter warnings when `require_frontmatter` is enabled

## Fix Findings by Priority

Work through the findings starting with critical issues:

1. **Critical issues first** -- fix incorrect code examples, wrong API references, and missing sections before anything else.
2. **Major issues next** -- update outdated content, fix broken links, and clarify ambiguous procedures.
3. **Minor issues last** -- correct formatting, fix typos, and standardize style.

If you want to fix a specific file interactively, use the cleanup command:

```
/doc-cleanup docs/api/endpoints.md
```

## Verify Your Fixes

After making corrections, re-run the review on the same scope to confirm the issues are resolved:

```
/doc-review docs/api/
```

Compare the new report against the original. The total issue count and file scores should reflect your improvements.

## Related

- [Commands reference](../reference/commands.md) -- full details on `/doc-review` arguments and behavior
- [Documentation quality](../explanation/documentation-quality.md) -- why these quality criteria matter
- [Configure your project](configure-project.md) -- customize which paths and standards the review uses
