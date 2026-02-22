---
title: "Agents"
diataxis_type: reference
---

# Agents

The plugin provides two autonomous agents that Claude Code can invoke proactively or on request.

## Summary

| Agent | Model | Color | Purpose |
|-------|-------|-------|---------|
| `doc-reviewer` | inherit | cyan | Audit documentation quality |
| `doc-writer` | inherit | magenta | Create and update documentation |

Both agents use the same tool set: `Read`, `Write`, `Edit`, `Glob`, `Grep`, `Bash`.

## doc-reviewer

Comprehensive documentation reviewer specializing in quality assurance.

**Model:** inherit (uses the current session model)
**Color:** cyan

**Activation triggers:**

- After significant code changes that may affect documentation
- When explicitly asked to review documentation
- During release preparation

**Process:**

| Phase | Activities |
|-------|-----------|
| Discovery | Locate documentation files; load project configuration from `.claude/documentation-review.local.md` |
| Accuracy Analysis | Compare documented APIs with implementation; verify code examples; check version numbers; validate configuration options; test internal links |
| Completeness Check | Verify all public APIs are documented; check for missing sections; identify undocumented features; flag placeholder or TODO content |
| Clarity Assessment | Evaluate heading structure; check for undefined terms; assess example quality; review for ambiguity |
| Formatting Review | Verify markdown syntax; check code block language tags; validate table formatting; assess style consistency |
| Diataxis Alignment | Classify documents by quadrant; check frontmatter; score mode purity; evaluate cross-references; apply quadrant-specific criteria |

**Output:** Structured review report containing:

- Scores (1--5) per document: accuracy, completeness, clarity, format, Diataxis alignment, overall
- Critical issues (must fix)
- Major issues (should fix)
- Minor issues (nice to fix)
- Improvement suggestions
- Prioritized action plan

**Scoring scale:**

| Score | Rating | Description |
|-------|--------|-------------|
| 5 | Excellent | Best practice example |
| 4 | Good | Minor improvements possible |
| 3 | Adequate | Meets basic requirements |
| 2 | Below standard | Significant issues |
| 1 | Poor | Major problems |

For a walkthrough, see the [Review Documentation guide](../guides/review-documentation.md).

## doc-writer

Expert technical writer for creating and restructuring documentation.

**Model:** inherit (uses the current session model)
**Color:** magenta

**Activation triggers:**

- When substantial documentation needs writing
- When documentation requires updating after significant changes
- When documentation needs restructuring

**Process:**

| Phase | Activities |
|-------|-----------|
| Research | Analyze codebase; review existing documentation; load project configuration; identify target audience |
| Planning | Determine document structure; identify key content; plan code examples; identify cross-references |
| Writing | Write content; include working code examples; add headings and formatting; cross-reference related docs |
| Validation | Verify code examples; check internal links; ensure terminology consistency; review against style guide |

**Documentation types:**

| Type | Contents |
|------|----------|
| README | Project overview, features, installation, basic usage, links to detailed docs |
| API docs | Endpoint/function descriptions, parameters, request/response examples, error handling |
| User guides | Step-by-step tutorials, use case walkthroughs, troubleshooting, best practices |
| Technical specs | Architecture overview, design decisions, integration points, performance considerations |

**Output:** Documentation file with a summary report covering:

- File path and document type
- Changes or sections created
- Recommended next steps

For a walkthrough, see the [Create Documentation guide](../guides/create-documentation.md).

## See also

- [Commands](commands.md) — the commands that agents back
- [Skills](skills.md) — knowledge modules loaded by agents
- [Configuration](configuration.md) — agent-relevant configuration keys
- [Review Documentation](../guides/review-documentation.md) — using the doc-reviewer agent in practice
- [Understanding Documentation Quality](../explanation/documentation-quality.md) — why automated review matters
