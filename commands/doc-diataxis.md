---
name: doc-diataxis
description: "Analyze documentation against the Diataxis framework (classify, review, gap analysis)"
argument-hint: "[classify|review|gaps|audit] [path]"
allowed-tools: Read, Glob, Grep, Bash(git:*), Bash(rg:*)
diataxis_type: reference
---


Analyze documentation against the Diátaxis framework.

## Operation Mode

$IF($1,
  Mode: $1
  $IF($2, Scope: @$2, Scope: all configured documentation paths),
  Ask the user which operation to perform:
  - **classify** — Classify documents into Diátaxis quadrants (tutorial, how-to, reference, explanation)
  - **review** — Review documents for Diátaxis quality criteria and mode purity
  - **gaps** — Identify missing documentation across Diátaxis quadrants per feature/component
  - **audit** — Full audit: classify + review + gaps in one pass
)

## Configuration Check

Use Glob tool to check if `.claude/documentation-review.local.md` exists.

If configuration exists, read it with Read tool and check for `diataxis:` section.
Use the following defaults if not configured:
- `strictness: standard`
- `require_frontmatter: true`
- `doc_paths:` from top-level plugin config, or require user to specify

## Operations

### Classify

For each documentation file in scope:

1. Read the file content
2. Check for `diataxis_type` in frontmatter
3. If no frontmatter, apply the Diátaxis compass:
   - Is the content action-oriented or cognition-oriented?
   - Is it acquisition (learning) or application (working)?
4. Assign the quadrant with confidence level (High/Medium/Low)
5. Detect mode mixing — sections where the document shifts quadrant
6. Report classification with recommendations

### Review

For each documentation file in scope:

1. Classify the document (if not already classified)
2. Apply the quadrant-specific quality criteria from the `diataxis` skill:
   - **Tutorial:** mode purity, learning path, reliability, tone, cross-references
   - **How-to:** mode purity, goal clarity, practical focus, sequencing, cross-references
   - **Reference:** mode purity, accuracy/completeness, structure, tone, cross-references
   - **Explanation:** mode purity, depth/context, connections, scope discipline, cross-references
3. Score each criterion using the weighted rubric
4. Calculate aggregate Diátaxis score
5. Flag mode mixing issues with specific locations

### Gaps

1. Inventory all documentation files
2. Classify each into its quadrant
3. Identify features/components documented (from file names, headings, content analysis)
4. Build a coverage matrix: feature × quadrant
5. Identify gaps: features missing documentation in one or more quadrants
6. Prioritize by user impact:
   - No tutorials → new users blocked
   - No how-to guides → users cannot solve problems
   - No reference → users cannot look up details
   - No explanation → users lack conceptual context

### Audit

Run all three operations in sequence: classify → review → gaps.

## Output Format

```markdown
# Diátaxis Analysis Report

**Date:** [date]
**Scope:** [files analyzed]
**Strictness:** [advisory | standard | strict]
**Operation:** [classify | review | gaps | audit]

## Classification Summary

| Document | Quadrant | Confidence | Frontmatter | Mode Mixing |
|----------|----------|------------|-------------|-------------|
| [file]   | Tutorial | High       | Yes         | None        |
| [file]   | How-to   | Medium     | No          | Lines 45-60 |

## Quality Scores (review mode)

| Document | Quadrant | Purity | [Criterion] | [Criterion] | Cross-ref | Score |
|----------|----------|--------|-------------|-------------|-----------|-------|
| [file]   | Tutorial | 4/5    | 5/5         | 3/5         | 2/5       | 3.8/5 |

## Coverage Matrix (gaps mode)

| Feature | Tutorial | How-to | Reference | Explanation |
|---------|----------|--------|-----------|-------------|
| [feat]  | Yes      | -      | Yes       | -           |

### Priority Gaps
1. [Feature]: Missing [quadrant] — [impact]
2. [Feature]: Missing [quadrant] — [impact]

## Mode Mixing Issues

### [filename]
- **Lines [X-Y]:** [Reference content in tutorial] — Recommend extracting to [target document]

## Recommendations

### Immediate Actions
1. [Most impactful fix]

### Structural Improvements
1. [Reorganization suggestion]

### Missing Documents to Create
1. [Document]: [quadrant] — [what it should cover]
```

Use the `diataxis` skill for detailed framework guidance, quality criteria, and classification rules.
