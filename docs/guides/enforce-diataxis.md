---
title: "How to Enforce the Diataxis Framework"
diataxis_type: how-to
---

# How to Enforce the Diataxis Framework

This guide explains how to use the documentation-review plugin to classify, audit, and enforce the Diataxis framework across your documentation.

## Prerequisites

- The documentation-review plugin installed (`claude plugin install zircote/documentation-review`)
- Documentation files to analyze

## Verify Diataxis Is Enabled

Diataxis analysis is enabled by default. If you want to confirm or adjust the settings, check your `.claude/documentation-review.local.md` configuration:

```yaml
diataxis:
  enabled: true
  strictness: strict
  require_frontmatter: true
```

If the `diataxis` section is missing, the defaults apply: enabled with `strict` strictness and frontmatter required.

## Choose a Strictness Level

Set the strictness level based on how strictly you want to enforce Diataxis compliance:

| Level | Behavior |
|-------|----------|
| `advisory` | Provides suggestions only; nothing is flagged as an issue |
| `standard` | Flags violations in the review report as issues |
| `strict` | Treats violations as errors that must be resolved |

If you want to start gently and tighten over time, begin with `advisory` and move to `standard` or `strict` as your documentation matures:

```yaml
diataxis:
  strictness: strict
```

## Run a Full Audit

If you want a complete picture of your documentation's Diataxis compliance, run:

```
/doc-diataxis audit
```

This performs three operations in sequence:

1. **Classify** -- assigns each document to a Diataxis quadrant (tutorial, how-to, reference, explanation)
2. **Review** -- scores each document against its quadrant's quality criteria
3. **Gaps** -- identifies features or components missing documentation in one or more quadrants

The output includes a classification table, quality scores, a coverage matrix, and prioritized recommendations.

## Classify Existing Documentation

If you only want to see how your documents map to Diataxis quadrants, run:

```
/doc-diataxis classify
```

Or scope it to a specific path:

```
/doc-diataxis classify docs/guides/
```

Each document receives a quadrant assignment with a confidence level (High, Medium, Low) and any mode mixing is flagged.

## Find Coverage Gaps

If you want to identify which features lack documentation in certain quadrants, run:

```
/doc-diataxis gaps
```

The output is a coverage matrix showing which features have tutorials, how-to guides, reference pages, and explanations -- and which are missing. Gaps are prioritized by user impact:

- Missing tutorials block new users from learning
- Missing how-to guides prevent users from solving problems
- Missing reference docs prevent users from looking up details
- Missing explanations leave users without conceptual context

## Fix Mode Mixing

Mode mixing occurs when a document blends content from multiple quadrants -- for example, a tutorial that includes reference tables, or a how-to guide that explains background theory.

The audit report identifies mode mixing by location (line ranges) and recommends where to extract the mixed content. To fix it:

1. Identify the mixed sections from the report.
2. Extract the off-quadrant content into a new document of the appropriate type.
3. Replace the extracted content with a cross-reference link to the new document.
4. Add `diataxis_type` frontmatter to both documents.

## Add Frontmatter to Documents

When `require_frontmatter` is enabled, each document should declare its quadrant in YAML frontmatter:

```yaml
---
title: "Document Title"
diataxis_type: tutorial    # tutorial, how-to, reference, or explanation
---
```

If you want to classify documents automatically and then apply the frontmatter, run `/doc-diataxis classify` first and use the results to add the correct type to each file.

## Organize by Quadrant Directories

If you want your directory structure to reflect the Diataxis quadrants, configure quadrant directories:

```yaml
diataxis:
  quadrant_directories:
    tutorials: docs/tutorials/
    how-to: docs/guides/
    reference: docs/reference/
    explanation: docs/explanation/
```

When set, the plugin validates that documents in each directory match the expected quadrant and flags misplaced files.

## Verify Compliance

After making changes, re-run the audit to confirm improvements:

```
/doc-diataxis audit
```

Compare the new classification table, quality scores, and gap analysis against the previous run.

## Related

- [Commands reference](../reference/commands.md) -- full details on `/doc-diataxis` subcommands
- [Diataxis framework](../explanation/diataxis-framework.md) -- background on the Diataxis approach and why it matters
- [Configure your project](configure-project.md) -- all configuration options including Diataxis settings
