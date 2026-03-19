# Documentation Classification Report

**Date:** 2026-03-19
**Scope:** Hypothetical project with 10 representative documentation files in `docs/`
**Framework:** Diataxis (Tutorials, How-To Guides, Reference, Explanation)

---

## Diataxis Categories

| Category | Purpose | Orientation |
|----------|---------|-------------|
| **Tutorial** | Learning-oriented. Walks a beginner through a series of steps to complete a project. | Practical + Studying |
| **How-To Guide** | Task-oriented. Provides steps to solve a specific real-world problem. | Practical + Working |
| **Reference** | Information-oriented. Describes the machinery: APIs, configs, CLIs. | Theoretical + Working |
| **Explanation** | Understanding-oriented. Clarifies concepts, architecture, design decisions. | Theoretical + Studying |

---

## File Classification

| File | Assigned Category | Confidence | Notes |
|------|-------------------|------------|-------|
| `getting-started.md` | Tutorial | High | Walks new users through first project setup end-to-end. Classic tutorial structure. |
| `quickstart.md` | Tutorial | Medium | Abbreviated tutorial. Lacks the depth of a full tutorial but follows the learning-oriented pattern. |
| `api-reference.md` | Reference | High | Exhaustive listing of API endpoints, parameters, return types. Pure reference material. |
| `configuration.md` | Reference | High | Documents all configuration options, environment variables, and defaults. |
| `architecture.md` | Explanation | High | Describes system design, component relationships, and design rationale. |
| `deployment.md` | How-To Guide | High | Step-by-step instructions for deploying to production. Goal-oriented, assumes prior knowledge. |
| `authentication.md` | How-To Guide | Medium | Covers setting up OAuth/API keys. Task-focused but mixes in conceptual explanation of auth flows. |
| `troubleshooting.md` | How-To Guide | High | Problem-solution pairs for common issues. Classic how-to structure. |
| `migration-guide.md` | How-To Guide | High | Guides users through upgrading from v1 to v2. Assumes familiarity with the system. |
| `faq.md` | Mixed | Low | Contains a blend of conceptual explanations, how-to snippets, and reference fragments. Needs splitting. |

---

## Classification Summary

| Category | Count | Files |
|----------|-------|-------|
| Tutorial | 2 | `getting-started.md`, `quickstart.md` |
| How-To Guide | 4 | `deployment.md`, `authentication.md`, `troubleshooting.md`, `migration-guide.md` |
| Reference | 2 | `api-reference.md`, `configuration.md` |
| Explanation | 1 | `architecture.md` |
| Mixed / Unclassified | 1 | `faq.md` |

---

## Gap Analysis

### 1. Explanation category is severely underrepresented (1 of 10 files)

The project has only `architecture.md` providing conceptual depth. Missing explanation-type docs include:

- **Security model explanation** -- `authentication.md` currently mixes how-to steps with conceptual material about auth flows. The conceptual portion should be extracted into a dedicated explanation doc.
- **Data model / domain concepts** -- No document explains the core domain objects, their relationships, or why the data model is structured the way it is.
- **Design decisions / ADRs** -- `architecture.md` covers structure but not the reasoning behind key trade-offs (e.g., why REST over GraphQL, why Postgres over DynamoDB).
- **Performance characteristics** -- No document explains caching behavior, connection pooling, or expected throughput characteristics.

### 2. Tutorial coverage is shallow

- `getting-started.md` covers initial setup but there is no tutorial for intermediate workflows (e.g., "Build your first integration" or "Create a custom plugin").
- `quickstart.md` is too abbreviated to function as a proper tutorial -- it skips context and explanation that a true beginner needs. Consider either expanding it into a full tutorial or reclassifying it as a how-to.

### 3. Reference docs are missing CLI documentation

- `api-reference.md` and `configuration.md` cover the HTTP API and config, but there is no CLI reference documenting available commands, flags, and exit codes.
- No reference doc for error codes and their meanings.
- No reference doc for data types, schemas, or database table structures.

### 4. How-To Guides have the best coverage but contain impurities

- `authentication.md` mixes explanation into its how-to steps. The conceptual material about OAuth flows and token lifecycle should be extracted to an explanation doc, leaving `authentication.md` as a pure task-oriented guide.
- `faq.md` is a catch-all that needs to be decomposed. Each FAQ entry likely belongs in one of the four Diataxis categories.

### 5. Missing cross-cutting docs

| Missing Document | Suggested Category | Priority |
|------------------|--------------------|----------|
| `cli-reference.md` | Reference | High |
| `error-codes.md` | Reference | Medium |
| `data-model.md` | Explanation | High |
| `design-decisions.md` | Explanation | Medium |
| `security-model.md` | Explanation | Medium |
| `build-first-integration.md` | Tutorial | High |
| `monitoring-setup.md` | How-To Guide | Medium |
| `backup-and-restore.md` | How-To Guide | Medium |

---

## Recommended Actions

1. **Split `faq.md`** -- Redistribute its content into the appropriate Diataxis categories. FAQ pages tend to accumulate because there is no better home for the content; creating proper homes eliminates the need for the FAQ.

2. **Extract explanation content from `authentication.md`** -- Move the OAuth flow explanation into a new `security-model.md` (Explanation). Leave `authentication.md` as a focused how-to.

3. **Create missing Reference docs** -- `cli-reference.md` and `error-codes.md` are high-value, low-effort additions since the information already exists in code.

4. **Add at least two Explanation docs** -- `data-model.md` and `design-decisions.md` will give contributors and advanced users the conceptual grounding that is currently absent.

5. **Add an intermediate Tutorial** -- `build-first-integration.md` bridges the gap between the beginner `getting-started.md` and the task-oriented how-to guides.

6. **Add Diataxis labels to each file** -- Include a front-matter field (e.g., `type: tutorial`) or a visible badge at the top of each doc so readers immediately know what kind of document they are reading.

---

## Balance Assessment

```
Current distribution:

Tutorial      ██████████░░░░░░░░░░  20%  (2 files)
How-To Guide  ████████████████████  40%  (4 files)
Reference     ██████████░░░░░░░░░░  20%  (2 files)
Explanation   █████░░░░░░░░░░░░░░░  10%  (1 file)
Mixed         █████░░░░░░░░░░░░░░░  10%  (1 file)

Target distribution (healthy project):

Tutorial      ██████████░░░░░░░░░░  20%
How-To Guide  ██████████████░░░░░░  30%
Reference     ██████████████░░░░░░  30%
Explanation   ██████████░░░░░░░░░░  20%
```

The primary imbalance is the deficit in Explanation and Reference content. How-To Guides are well-covered. Tutorials are adequate for a project of this size but would benefit from an intermediate-level addition.
