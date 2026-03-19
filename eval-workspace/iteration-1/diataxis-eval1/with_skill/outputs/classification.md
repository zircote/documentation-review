# Diataxis Classification Report

**Project:** Hypothetical Web Application Framework (simulated)
**Date:** 2026-03-19
**Strictness:** strict
**Documents analyzed:** 10

---

## Methodology

Each document was evaluated using the Diataxis compass — two diagnostic questions:

1. Does this content involve **practical steps** (action) or **theoretical knowledge** (cognition)?
2. Is the user **learning something new** (acquisition) or **applying existing skill** (application)?

The intersection places every document in exactly one quadrant:

|                | **Acquisition** (learning) | **Application** (doing) |
|----------------|----------------------------|-------------------------|
| **Action**     | Tutorial                   | How-to Guide            |
| **Cognition**  | Explanation                | Reference               |

---

## Document Classification

### 1. `docs/getting-started.md`

- **Classified as:** Tutorial
- **Compass:** Action + Acquisition. Walks a brand-new user through installing the framework, creating a first project, and running the dev server. Learning happens through doing.
- **Mode purity:** MINOR ISSUE — Lines 78-95 drift into an explanation of the project directory structure and why certain conventions exist. This "why" content belongs in an explanation document.
- **Recommended frontmatter:**
  ```yaml
  diataxis_type: tutorial
  diataxis_learning_goals:
    - Install the framework and verify a working environment
    - Create and run a minimal project
  ```

### 2. `docs/api-reference.md`

- **Classified as:** Reference
- **Compass:** Cognition + Application. Lists every public API endpoint with parameters, return types, status codes, and examples. Consulted during active development, not read end-to-end.
- **Mode purity:** CLEAN. Factual, austere, structured by the API surface. No narrative or instruction.
- **Recommended frontmatter:**
  ```yaml
  diataxis_type: reference
  diataxis_describes: REST API endpoints (v2)
  ```

### 3. `docs/architecture.md`

- **Classified as:** Explanation
- **Compass:** Cognition + Acquisition. Discusses the event-driven architecture, why the team chose message queues over direct RPC, and how the service mesh evolved. Provides context, history, and trade-offs.
- **Mode purity:** CLEAN. Discursive, reflective, no step-by-step instructions.
- **Recommended frontmatter:**
  ```yaml
  diataxis_type: explanation
  diataxis_topic: System architecture and design rationale
  ```

### 4. `docs/deployment.md`

- **Classified as:** How-to Guide
- **Compass:** Action + Application. Directs a competent user through deploying to production — assumes familiarity with Docker, cloud providers, and CLI tools. Goal-oriented, not a learning exercise.
- **Mode purity:** MAJOR ISSUE — Section "Understanding Load Balancer Health Checks" (lines 42-68) is explanation content embedded in a procedural guide. Should be extracted to `docs/explanation/health-checks.md` and linked.
- **Recommended frontmatter:**
  ```yaml
  diataxis_type: how-to
  diataxis_goal: Deploy the application to a production environment
  ```

### 5. `docs/configuration.md`

- **Classified as:** Reference (partially mixed)
- **Compass:** Primarily Cognition + Application — describes every configuration key, type, default, and valid range. However, the document opens with a 20-line narrative about "why configuration matters" and closes with step-by-step "How to override config in CI."
- **Mode purity:** CRITICAL — Three modes in one document. The opening narrative is explanation; the closing section is a how-to guide; the middle is reference.
- **Recommended split:**
  - Extract opening to `docs/explanation/configuration-philosophy.md`
  - Extract closing to `docs/guides/override-config-in-ci.md`
  - Keep the parameter table as `docs/reference/configuration.md`
- **Recommended frontmatter (after split):**
  ```yaml
  diataxis_type: reference
  diataxis_describes: Configuration keys and environment variables
  ```

### 6. `docs/authentication.md`

- **Classified as:** How-to Guide
- **Compass:** Action + Application. Shows a competent user how to set up OAuth2, configure token lifetimes, and integrate with an identity provider. Task-oriented.
- **Mode purity:** MINOR ISSUE — Inline note explaining the OAuth2 authorization code flow (lines 30-45) is explanation content. Link to a dedicated explanation document instead.
- **Recommended frontmatter:**
  ```yaml
  diataxis_type: how-to
  diataxis_goal: Configure OAuth2 authentication
  ```

### 7. `docs/troubleshooting.md`

- **Classified as:** How-to Guide
- **Compass:** Action + Application. Problem/solution format — each section names a specific error and gives steps to resolve it. The user arrives with a goal (fix this error) and leaves with a solution.
- **Mode purity:** CLEAN. Direct, practical, no teaching or narrative.
- **Recommended frontmatter:**
  ```yaml
  diataxis_type: how-to
  diataxis_goal: Diagnose and resolve common errors
  ```

### 8. `docs/database-setup.md`

- **Classified as:** Tutorial
- **Compass:** Action + Acquisition. Guides a new user through provisioning a database, running migrations, and seeding sample data. Uses "we" language and verifies output at every step.
- **Mode purity:** MINOR ISSUE — Lines 90-102 list every migration CLI flag with descriptions. This is reference material. Extract to `docs/reference/migration-cli.md`.
- **Recommended frontmatter:**
  ```yaml
  diataxis_type: tutorial
  diataxis_learning_goals:
    - Provision and connect a local database
    - Run migrations and seed data
  ```

### 9. `docs/plugin-development.md`

- **Classified as:** How-to Guide (misidentified as tutorial by filename convention)
- **Compass:** Action + Application. Despite the title "Building Your First Plugin," the document assumes familiarity with the plugin API and jumps straight to implementation without learning scaffolding. The user has a concrete goal (build a plugin), not a learning objective.
- **Mode purity:** MAJOR ISSUE — Mixes how-to steps with reference tables of hook types (lines 55-80) and an explanation section on "Why Plugins Use Isolated Contexts" (lines 120-140). Three modes present.
- **Recommended split:**
  - Rewrite as a focused how-to: `docs/guides/create-a-plugin.md`
  - Extract hook reference: `docs/reference/plugin-hooks.md`
  - Extract isolation explanation: `docs/explanation/plugin-isolation.md`
- **Recommended frontmatter (after split):**
  ```yaml
  diataxis_type: how-to
  diataxis_goal: Create and register a custom plugin
  ```

### 10. `docs/changelog.md`

- **Classified as:** Not classifiable (outside Diataxis scope)
- **Notes:** Changelogs are operational artifacts, not user documentation. They do not fit any of the four quadrants. No Diataxis frontmatter needed.

---

## Diataxis Coverage Matrix

| Feature/Component    | Tutorial | How-to | Reference | Explanation |
|----------------------|----------|--------|-----------|-------------|
| Getting Started      | YES      | --     | --        | --          |
| API                  | --       | --     | YES       | --          |
| Architecture         | --       | --     | --        | YES         |
| Deployment           | --       | YES    | --        | --          |
| Configuration        | --       | (mixed)| YES*      | (mixed)     |
| Authentication       | --       | YES    | --        | --          |
| Troubleshooting      | --       | YES    | --        | --          |
| Database             | YES      | --     | --        | --          |
| Plugin Development   | --       | YES*   | (mixed)   | (mixed)     |

*Asterisk indicates the document requires splitting before the classification is clean.*

---

## Gap Analysis

### Critical Gaps (high user impact)

1. **No tutorials for Authentication or Deployment.** New users adopting the framework will hit authentication and deployment early. Without learning-oriented guides, they fall back on the how-to docs, which assume competence they may not yet have.
   - **Recommendation:** Create `docs/tutorials/first-auth-setup.md` and `docs/tutorials/first-deployment.md`.

2. **No reference documentation for Deployment, Authentication, or Database.** Competent users deploying or configuring auth have no place to look up exact parameter names, environment variables, or CLI flags.
   - **Recommendation:** Create `docs/reference/deployment-options.md`, `docs/reference/auth-config.md`, `docs/reference/database-cli.md`.

3. **No how-to guides for API usage.** The API has reference docs but no task-oriented guides (e.g., "How to paginate results," "How to handle rate limiting").
   - **Recommendation:** Create 3-5 targeted how-to guides for the most common API tasks.

### Moderate Gaps

4. **No explanation documents for Authentication, Configuration, Database, or Deployment.** Users cannot understand *why* things work the way they do for most features. Only Architecture has explanation coverage.
   - **Recommendation:** Prioritize `docs/explanation/auth-model.md` and `docs/explanation/configuration-philosophy.md` (the latter can be extracted from the mixed `configuration.md`).

5. **Getting Started has no companion how-to or reference.** After completing the tutorial, users have nowhere to go for quick task-oriented follow-ups or to look up what they learned.
   - **Recommendation:** Create `docs/guides/common-first-tasks.md` and ensure the tutorial links to relevant reference pages.

### Low Priority Gaps

6. **No tutorial for Plugin Development.** Plugin authors are typically experienced developers; a how-to guide is sufficient. A tutorial would be nice-to-have but is not critical.

---

## Mode Mixing Summary

| Document              | Severity | Modes Mixed             | Action Required           |
|-----------------------|----------|-------------------------|---------------------------|
| `configuration.md`    | CRITICAL | Reference + Explanation + How-to | Split into 3 documents |
| `plugin-development.md` | MAJOR | How-to + Reference + Explanation | Split into 3 documents |
| `deployment.md`       | MAJOR    | How-to + Explanation     | Extract explanation section |
| `getting-started.md`  | MINOR    | Tutorial + Explanation   | Extract or trim explanation |
| `authentication.md`   | MINOR    | How-to + Explanation     | Extract inline explanation  |
| `database-setup.md`   | MINOR    | Tutorial + Reference     | Extract CLI reference table |

---

## Cross-Reference Audit

Documents were checked for outgoing links to other Diataxis quadrants.

| Document               | Links to Tutorial | Links to How-to | Links to Reference | Links to Explanation | Verdict      |
|------------------------|-------------------|-----------------|--------------------|--------------------|--------------|
| `getting-started.md`   | --                | No              | No                 | No                 | FAIL: isolated |
| `api-reference.md`     | --                | No              | --                 | No                 | FAIL: isolated |
| `architecture.md`      | No                | No              | No                 | --                 | FAIL: isolated |
| `deployment.md`        | No                | --              | No                 | No                 | FAIL: isolated |
| `configuration.md`     | No                | (internal)      | (internal)         | (internal)         | FAIL: mixed, not linked |
| `authentication.md`    | No                | --              | No                 | No                 | FAIL: isolated |
| `troubleshooting.md`   | No                | --              | No                 | No                 | FAIL: isolated |
| `database-setup.md`    | --                | No              | No                 | No                 | FAIL: isolated |
| `plugin-development.md`| No                | (internal)      | (internal)         | (internal)         | FAIL: mixed, not linked |

**Result:** Zero documents have proper cross-references to other quadrants. Every document is an island. This is the single most impactful structural improvement available: adding a "See also" or "Related" section to each document that connects it to its sibling quadrants.

---

## Prioritized Recommendations

### Immediate (week 1)

1. **Split `configuration.md` and `plugin-development.md`** into their component quadrants. These two documents account for the worst mode mixing and would immediately produce 4 additional clean, classifiable documents.
2. **Add cross-reference sections** to all 10 documents. Even placeholder links to "future" docs establish the navigational skeleton.
3. **Add `diataxis_type` frontmatter** to every document.

### Short-term (weeks 2-3)

4. **Create missing reference docs** for Deployment, Authentication, and Database. Competent users need lookup material.
5. **Create 3-5 API how-to guides** for the most common tasks. The API reference alone is insufficient for users who know what they want to do but not how.
6. **Extract explanation content** from `deployment.md`, `authentication.md`, and `getting-started.md` into standalone explanation documents.

### Medium-term (month 2)

7. **Create tutorials for Authentication and Deployment.** These fill the most impactful learning gaps.
8. **Create explanation documents** for Configuration and Database to complete the understanding layer.
9. **Establish quadrant directory structure:**
   ```
   docs/
     tutorials/
     guides/
     reference/
     explanation/
   ```

---

## Quadrant Distribution Summary

| Quadrant    | Count | Percentage | Health   |
|-------------|-------|------------|----------|
| Tutorial    | 2     | 22%        | Weak     |
| How-to      | 4     | 44%        | Moderate |
| Reference   | 2     | 22%        | Weak     |
| Explanation  | 1    | 11%        | Critical |
| Unclassifiable | 1  | --         | N/A      |

The documentation set is heavily skewed toward how-to guides, which is common in projects where documentation is written reactively (users ask "how do I do X?" and someone writes an answer). The explanation quadrant is nearly empty, meaning users have no way to build conceptual understanding. Tutorials are thin, meaning onboarding relies on users already knowing enough to follow how-to guides.

A healthy documentation set trends toward roughly equal coverage across all four quadrants, adjusted for the project's maturity and audience.
