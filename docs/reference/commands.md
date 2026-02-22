---
title: "Commands"
diataxis_type: reference
---

# Commands

The documentation-review plugin provides six slash commands. Each command is invoked with `/command-name` in Claude Code.

## Summary

| Command | Description | Argument | Diataxis Type |
|---------|-------------|----------|---------------|
| `/doc-review` | Review documentation for quality, accuracy, and completeness | `[path]` | reference |
| `/doc-create` | Generate new documentation from codebase analysis | `<type: readme\|api\|template>` | how-to |
| `/doc-update` | Update outdated documentation with current information | `[path]` | how-to |
| `/doc-cleanup` | Identify and report obsolete or outdated documentation | `[path]` | reference |
| `/doc-setup` | Interactive setup for documentation review configuration | `[--update]` | how-to |
| `/doc-diataxis` | Analyze documentation against the Diataxis framework | `[classify\|review\|gaps\|audit] [path]` | reference |

## doc-review

Review documentation for quality, accuracy, and completeness.

**Argument:** `[path]` (optional — defaults to configured `doc_paths`)

**Allowed tools:** `Read`, `Glob`, `Grep`, `Bash(git:*)`, `Bash(rg:*)`

**Output:** Markdown report containing:

- Scores (1--5) per document across accuracy, completeness, clarity, format, and Diataxis alignment
- Findings grouped by severity: critical, major, minor, suggestions
- Prioritized action items

This command does not modify files. For usage, see the [Review Documentation guide](../guides/review-documentation.md).

## doc-create

Generate new documentation from codebase analysis.

**Argument:** `<type: readme|api|template>` (required)

**Allowed tools:** `Read`, `Write`, `Glob`, `Grep`, `Bash(git:*)`, `Bash(rg:*)`, `Bash(mkdir:*)`, `Bash(cat:*)`, `Bash(uuidgen:*)`

**Output:** Generated documentation file with a summary report covering:

- File path and type created
- Sections included
- Recommendations for next steps

For usage, see the [Create Documentation guide](../guides/create-documentation.md).

## doc-update

Update outdated documentation with current information.

**Argument:** `[path]` (optional — defaults to configured `doc_paths`)

**Allowed tools:** `Read`, `Write`, `Edit`, `Glob`, `Grep`, `Bash(git:*)`, `Bash(rg:*)`

**Output:** Update report containing:

- Changes applied with before/after diffs
- Files modified
- Remaining issues flagged for manual review

For usage, see the [Review Documentation guide](../guides/review-documentation.md).

## doc-cleanup

Identify and report obsolete or outdated documentation.

**Argument:** `[path]` (optional — defaults to configured `doc_paths`)

**Allowed tools:** `Read`, `Glob`, `Grep`, `Bash(git:*)`, `Bash(rg:*)`

**Output:** Cleanup report containing:

- Obsolete files with confidence levels (high, medium, low)
- Broken references and dead links
- Duplicate content across files

This command reports only. It does not delete files. For usage, see the [Review Documentation guide](../guides/review-documentation.md).

## doc-setup

Interactive setup for documentation review configuration.

**Argument:** `[--update]` (optional — update existing configuration)

**Allowed tools:** `Read`, `Write`, `Glob`, `Bash(mkdir:*)`, `Bash(rg:*)`, `Bash(cat:*)`, `Bash(uuidgen:*)`

**Output:** Creates `.claude/documentation-review.local.md` with detected settings including:

- Documentation paths auto-detected from the project
- Ignore patterns
- Standards and API documentation settings
- Site generator and Diataxis configuration

For usage, see the [Configure Project guide](../guides/configure-project.md).

## doc-diataxis

Analyze documentation against the Diataxis framework.

**Argument:** `[classify|review|gaps|audit] [path]` (optional — defaults to `classify` with configured `doc_paths`)

**Allowed tools:** `Read`, `Glob`, `Grep`, `Bash(git:*)`, `Bash(rg:*)`

**Modes:**

| Mode | Description |
|------|-------------|
| `classify` | Classify each document into a Diataxis quadrant |
| `review` | Review documents against quadrant-specific quality criteria |
| `gaps` | Identify missing documentation across the four quadrants |
| `audit` | Full audit combining classification, review, and gap analysis |

**Output:** Diataxis analysis report containing:

- Document classification by quadrant (tutorial, how-to, reference, explanation)
- Quality scores per quadrant criteria
- Coverage matrix across all four quadrants
- Mode mixing issues (content drifting into another quadrant)

This command does not modify files. For usage, see the [Enforce Diataxis guide](../guides/enforce-diataxis.md).

## See also

- [Skills](skills.md) — knowledge modules loaded by commands
- [Agents](agents.md) — agents that execute command workflows
- [Configuration](configuration.md) — settings that control command behavior
- [Getting Started](../tutorials/getting-started.md) — first encounter with commands
