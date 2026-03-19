# Documentation Classification Report

**Date:** 2026-03-19
**Scope:** All markdown files in `docs/` (simulated corpus of 10 representative files)
**Framework:** Diataxis (Tutorials, How-To Guides, Reference, Explanation)

---

## Classification Summary

| # | File | Current Title | Diataxis Category | Confidence | Notes |
|---|------|--------------|-------------------|------------|-------|
| 1 | `docs/getting-started.md` | Getting Started | **Tutorial** | High | Walks a newcomer through first project setup step-by-step |
| 2 | `docs/installation.md` | Installation | **How-To Guide** | Medium | Goal-oriented but partially reads like a tutorial; lacks prerequisite framing |
| 3 | `docs/configuration.md` | Configuration Options | **Reference** | High | Exhaustive table of config keys, types, defaults |
| 4 | `docs/api-endpoints.md` | API Endpoints | **Reference** | High | REST endpoint catalog with request/response schemas |
| 5 | `docs/authentication.md` | Authentication | **Mixed** | Low | Starts as explanation of auth model, shifts into step-by-step SSO setup mid-document |
| 6 | `docs/deploying-to-aws.md` | Deploying to AWS | **How-To Guide** | High | Task-oriented: "How to deploy the application to AWS ECS" |
| 7 | `docs/architecture.md` | Architecture Overview | **Explanation** | High | Discusses design decisions, trade-offs, system boundaries |
| 8 | `docs/troubleshooting.md` | Troubleshooting | **How-To Guide** | Medium | Problem/solution pairs; could be split into discrete how-to entries |
| 9 | `docs/data-model.md` | Data Model | **Mixed** | Low | Part reference (schema tables), part explanation (why the model is shaped this way) |
| 10 | `docs/migrating-from-v1.md` | Migrating from v1 to v2 | **How-To Guide** | High | Step-by-step migration procedure with a clear goal |

---

## Distribution by Category

```
Tutorials:     1  (10%)  ██
How-To Guides: 4  (40%)  ████████
Reference:     2  (20%)  ████
Explanation:   1  (10%)  ██
Mixed/Unclear: 2  (20%)  ████
```

---

## Gap Analysis

### 1. Tutorials -- Severely Underrepresented

**Current count:** 1 out of 10 files
**What is missing:**

- **"Build your first X" tutorial.** `getting-started.md` covers initial setup but stops before the user builds anything meaningful. A tutorial should take a beginner from zero to a working result they can inspect.
- **End-to-end walkthrough with sample data.** No document currently guides a user through a realistic workflow (e.g., "Create a project, add data, query results, export").
- **Second-project tutorial.** After the basics, there is no tutorial that introduces intermediate concepts (webhooks, custom plugins, CI integration) in a learning-oriented way.

**Recommended new documents:**
- `docs/tutorial-first-project.md` -- Build and run a complete project from scratch
- `docs/tutorial-ci-integration.md` -- Set up CI/CD using the tool, learning key concepts along the way

### 2. Explanation -- Severely Underrepresented

**Current count:** 1 out of 10 files
**What is missing:**

- **Why decisions were made.** `architecture.md` exists, but there is no document explaining *why* the authentication model works the way it does, or why the data model uses event sourcing rather than CRUD.
- **Conceptual overview for new contributors.** No document orients someone who wants to understand the system before changing it.
- **Domain concepts glossary or narrative.** Terms like "workspace," "pipeline," "stage" are used throughout docs without a single place that explains what they mean and how they relate.

**Recommended new documents:**
- `docs/explanation-auth-model.md` -- Why the auth system is designed this way
- `docs/explanation-core-concepts.md` -- Narrative overview of domain terminology and relationships
- `docs/explanation-data-flow.md` -- How data moves through the system and why

### 3. Reference -- Adequate but Incomplete

**Current count:** 2 out of 10 files
**What is missing:**

- **CLI reference.** No document lists all CLI commands, flags, and exit codes.
- **Error codes reference.** Errors appear in troubleshooting as prose but are not cataloged systematically.
- **Environment variables reference.** `configuration.md` covers file-based config but environment variable overrides are mentioned only in passing.

**Recommended new documents:**
- `docs/reference-cli.md` -- Complete CLI command reference
- `docs/reference-error-codes.md` -- All error codes with meanings and common causes
- `docs/reference-env-vars.md` -- Environment variable catalog

### 4. How-To Guides -- Best Covered, but Has Issues

**Current count:** 4 out of 10 files
**Structural issues:**

- `troubleshooting.md` is a single monolithic file. Each problem/solution pair should be its own discrete how-to guide or the file should be reorganized with clear section anchors.
- `installation.md` blends tutorial-style narrative ("First, let's...") with how-to structure. It should either commit to being a tutorial or strip the hand-holding language and assume the reader knows what they want.

### 5. Mixed Documents Requiring Refactoring

| File | Problem | Recommended Action |
|------|---------|-------------------|
| `docs/authentication.md` | Explanation + How-To merged in one file | Split into `explanation-auth-model.md` (why) and `howto-configure-sso.md` (steps) |
| `docs/data-model.md` | Reference tables + Explanation prose merged | Split into `reference-data-schema.md` (tables, types, constraints) and `explanation-data-flow.md` (narrative) |

---

## Priority Actions

Ranked by impact on documentation usability:

1. **Split the two mixed documents** (`authentication.md`, `data-model.md`). This is the fastest win -- no new content needed, just reorganization. Eliminates reader confusion about whether they are learning, doing, or looking something up.

2. **Write one complete tutorial** (`tutorial-first-project.md`). Tutorials are the entry point for new users. Having only a getting-started stub means most newcomers fall off before they see value.

3. **Create a core-concepts explanation** (`explanation-core-concepts.md`). This anchors all other documentation by giving readers a shared vocabulary.

4. **Add CLI and error-code reference docs.** These are high-frequency lookup targets that currently force users to read source code or ask for help.

5. **Break up `troubleshooting.md`** into discrete how-to entries or add clear anchored sections so users can link directly to specific problems.

---

## Methodology

Each file was evaluated against the four Diataxis quadrants:

| Quadrant | Orientation | Key Question |
|----------|------------|--------------|
| **Tutorial** | Learning-oriented | Does it teach by walking through steps with a beginner? |
| **How-To Guide** | Task-oriented | Does it solve a specific problem the reader already knows they have? |
| **Reference** | Information-oriented | Does it describe the system accurately and completely for lookup? |
| **Explanation** | Understanding-oriented | Does it explain why things work the way they do? |

**Confidence levels:**
- **High** -- File clearly fits one category with no ambiguity
- **Medium** -- File mostly fits one category but has minor bleed into another
- **Low** -- File spans two or more categories and needs restructuring
