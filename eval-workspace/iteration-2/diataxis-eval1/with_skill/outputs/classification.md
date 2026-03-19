# Diataxis Documentation Classification & Gap Analysis

**Project:** Hypothetical API Platform (sample analysis)
**Date:** 2026-03-19
**Strictness level:** strict
**Documents analyzed:** 10

---

## Classification Method

Each document was classified using the Diataxis compass — two diagnostic questions:

1. Does this content involve **practical steps** (action) or **theoretical knowledge** (cognition)?
2. Is the user **learning something new** (acquisition) or **applying existing skill** (application)?

|                | **Acquisition** (learning) | **Application** (doing) |
|----------------|----------------------------|-------------------------|
| **Action**     | Tutorial                   | How-to Guide            |
| **Cognition**  | Explanation                | Reference               |

---

## Document-by-Document Classification

### 1. `docs/getting-started.md`

- **Classified as:** Tutorial
- **Confidence:** High
- **Compass reasoning:** Action + Acquisition. Walks a new user through creating their first API endpoint step by step, with expected output shown at each stage.
- **Mode purity:** Mixed (score: 6/10). Contains a 15-line reference table of all configuration options mid-tutorial that breaks learner momentum. Also includes a "Why REST?" digression (explanation content).
- **Recommended frontmatter:**
  ```yaml
  diataxis_type: tutorial
  diataxis_learning_goals:
    - Create a basic API endpoint
    - Understand the request/response cycle
  ```
- **Recommended splits:**
  - Extract configuration options table to `docs/reference/configuration.md`
  - Extract "Why REST?" section to `docs/explanation/rest-design-philosophy.md`

---

### 2. `docs/authentication.md`

- **Classified as:** How-to Guide
- **Confidence:** Medium
- **Compass reasoning:** Action + Application. Describes how to set up OAuth2, JWT, and API key authentication for someone who already understands auth concepts.
- **Mode purity:** Mixed (score: 5/10). Opens with two pages of conceptual explanation about OAuth2 flows (explanation content). Middle section is solid how-to. Ends with a full parameter reference table (reference content).
- **Recommended frontmatter:**
  ```yaml
  diataxis_type: how-to
  diataxis_goal: Configure authentication for your API
  ```
- **Recommended splits:**
  - Extract OAuth2 conceptual overview to `docs/explanation/authentication-concepts.md`
  - Extract parameter tables to `docs/reference/auth-api.md`

---

### 3. `docs/api-reference.md`

- **Classified as:** Reference
- **Confidence:** High
- **Compass reasoning:** Cognition + Application. Lists all endpoints, parameters, return types, and error codes. Structured by resource, mirroring the API itself.
- **Mode purity:** Good (score: 8/10). A few endpoints include mini how-to steps ("To paginate results, first call...") that should be extracted.
- **Recommended frontmatter:**
  ```yaml
  diataxis_type: reference
  diataxis_describes: REST API endpoints
  ```
- **Recommended splits:**
  - Extract pagination/filtering how-to steps to `docs/guides/how-to-paginate-results.md`

---

### 4. `docs/configuration.md`

- **Classified as:** How-to Guide
- **Confidence:** Medium
- **Compass reasoning:** Action + Application. Primarily tells users how to configure the system for specific scenarios (Docker, Kubernetes, bare metal).
- **Mode purity:** Mixed (score: 5/10). Mixes task-oriented setup instructions with an exhaustive table of every config key (reference content). The table dominates the document.
- **Recommended frontmatter:**
  ```yaml
  diataxis_type: how-to
  diataxis_goal: Configure the platform for your deployment environment
  ```
- **Recommended splits:**
  - Extract full config key reference table to `docs/reference/configuration-options.md`

---

### 5. `docs/architecture.md`

- **Classified as:** Explanation
- **Confidence:** High
- **Compass reasoning:** Cognition + Acquisition. Discusses why the platform uses an event-driven architecture, compares alternatives (monolith, microservices), and provides historical context for design decisions.
- **Mode purity:** Good (score: 9/10). Clean explanation content. One paragraph near the end instructs readers to "run `platform info` to see your current topology" — minor how-to drift.
- **Recommended frontmatter:**
  ```yaml
  diataxis_type: explanation
  diataxis_topic: Platform architecture and design decisions
  ```
- **Recommended action:** Remove the instructional paragraph or link to a how-to guide.

---

### 6. `docs/deployment.md`

- **Classified as:** How-to Guide
- **Confidence:** High
- **Compass reasoning:** Action + Application. Step-by-step instructions for deploying to production, aimed at someone who already knows the platform.
- **Mode purity:** Good (score: 8/10). Stays focused on the deployment task. Minor drift into explaining why blue-green deploys are preferred (explanation content).
- **Recommended frontmatter:**
  ```yaml
  diataxis_type: how-to
  diataxis_goal: Deploy the platform to production
  ```
- **Recommended action:** Move blue-green deployment rationale to an explanation doc or link to `docs/architecture.md`.

---

### 7. `docs/troubleshooting.md`

- **Classified as:** How-to Guide
- **Confidence:** High
- **Compass reasoning:** Action + Application. Structured as problem/solution pairs for a competent user encountering specific issues.
- **Mode purity:** Good (score: 8/10). Clean conditional-imperative style ("If you see error X, do Y"). Some entries include root-cause explanations that could be extracted but are short enough to be acceptable.
- **Recommended frontmatter:**
  ```yaml
  diataxis_type: how-to
  diataxis_goal: Diagnose and resolve common platform issues
  ```

---

### 8. `docs/data-modeling.md`

- **Classified as:** Explanation
- **Confidence:** Medium
- **Compass reasoning:** Cognition + Acquisition. Discusses the data modeling philosophy, why certain patterns were chosen, and trade-offs with alternatives.
- **Mode purity:** Mixed (score: 6/10). Includes a full schema reference table in the middle (reference content) and a "quick start" section that reads like a tutorial.
- **Recommended frontmatter:**
  ```yaml
  diataxis_type: explanation
  diataxis_topic: Data modeling philosophy and patterns
  ```
- **Recommended splits:**
  - Extract schema reference to `docs/reference/schema.md`
  - Extract quick-start walkthrough to `docs/tutorials/first-data-model.md`

---

### 9. `docs/cli-reference.md`

- **Classified as:** Reference
- **Confidence:** High
- **Compass reasoning:** Cognition + Application. Lists all CLI commands, flags, and options in a structured format mirroring the CLI's own help output.
- **Mode purity:** Excellent (score: 10/10). Pure reference content with concise usage examples per command. No mode mixing detected.
- **Recommended frontmatter:**
  ```yaml
  diataxis_type: reference
  diataxis_describes: CLI commands and options
  ```

---

### 10. `docs/plugins.md`

- **Classified as:** Unclassifiable (severe mode mixing)
- **Confidence:** Low
- **Compass reasoning:** This document attempts to serve all four quadrants simultaneously. It opens with a tutorial walkthrough of building a plugin, transitions into a how-to for publishing plugins, includes a full plugin API reference table, and ends with an essay on plugin architecture philosophy.
- **Mode purity:** Poor (score: 2/10). Four-way mode mixing makes this document difficult for every audience.
- **Recommended splits (priority):**
  - `docs/tutorials/build-your-first-plugin.md` — Tutorial
  - `docs/guides/how-to-publish-a-plugin.md` — How-to Guide
  - `docs/reference/plugin-api.md` — Reference
  - `docs/explanation/plugin-architecture.md` — Explanation

---

## Classification Summary

| Document | Quadrant | Purity Score | Needs Split? |
|----------|----------|:------------:|:------------:|
| `getting-started.md` | Tutorial | 6/10 | Yes |
| `authentication.md` | How-to Guide | 5/10 | Yes |
| `api-reference.md` | Reference | 8/10 | Minor |
| `configuration.md` | How-to Guide | 5/10 | Yes |
| `architecture.md` | Explanation | 9/10 | No |
| `deployment.md` | How-to Guide | 8/10 | No |
| `troubleshooting.md` | How-to Guide | 8/10 | No |
| `data-modeling.md` | Explanation | 6/10 | Yes |
| `cli-reference.md` | Reference | 10/10 | No |
| `plugins.md` | Unclassifiable | 2/10 | Yes (critical) |

**Quadrant distribution (current 10 documents):**
- Tutorials: 1
- How-to Guides: 4
- Reference: 2
- Explanation: 2
- Unclassifiable: 1

---

## Diataxis Coverage Matrix

Mapping documentation coverage against the platform's major features/components:

| Feature/Component | Tutorial | How-to | Reference | Explanation |
|-------------------|:--------:|:------:|:---------:|:-----------:|
| Getting Started | Yes | - | - | - |
| Authentication | - | Yes | - | - |
| API Endpoints | - | - | Yes | - |
| Configuration | - | Yes | - | - |
| Architecture | - | - | - | Yes |
| Deployment | - | Yes | - | - |
| Troubleshooting | - | Yes | - | - |
| Data Modeling | - | - | - | Yes |
| CLI | - | - | Yes | - |
| Plugins | - | - | - | - |

**Legend:** "Yes" = dedicated, mode-pure content exists. "-" = no coverage in this quadrant.

---

## Gap Analysis

### Gaps by Feature

**Authentication**
- Missing: Tutorial (new users need a guided auth setup experience)
- Missing: Reference (full auth parameter/endpoint listing)
- Missing: Explanation (OAuth2 concepts, token lifecycle, security model)
- Priority: HIGH -- authentication is a barrier for every new user

**API Endpoints**
- Missing: Tutorial (making your first API call)
- Missing: How-to (common integration patterns, filtering, pagination)
- Missing: Explanation (API design philosophy, versioning strategy)
- Priority: HIGH -- the API is the core product surface

**Configuration**
- Missing: Tutorial (basic configuration walkthrough for first-time setup)
- Missing: Reference (full config key listing with types, defaults, constraints)
- Missing: Explanation (why certain defaults were chosen, config philosophy)
- Priority: MEDIUM -- users need the reference table extracted from the current how-to

**Deployment**
- Missing: Tutorial (deploy-your-first-instance guided walkthrough)
- Missing: Reference (deployment configuration options, environment variables)
- Missing: Explanation (deployment architecture decisions, scaling rationale)
- Priority: MEDIUM

**CLI**
- Missing: Tutorial (CLI basics for new users)
- Missing: How-to (common CLI workflows and recipes)
- Missing: Explanation (CLI design philosophy)
- Priority: LOW -- reference is the most critical need for CLI, and it is well-covered

**Plugins**
- Missing: All four quadrants (current document must be split)
- Priority: HIGH -- the current doc serves nobody well

**Getting Started**
- Missing: How-to (quick setup for experienced users who do not need the tutorial)
- Missing: Reference (system requirements, supported platforms)
- Missing: Explanation (platform overview, key concepts)
- Priority: MEDIUM

### Gaps by Quadrant

**Tutorials (most critical gap)**
Currently only 1 tutorial exists across 7 features. New users cannot get started with authentication, the API, deployment, the CLI, or plugins through guided learning experiences.

- Recommended new tutorials:
  1. `tutorials/authenticate-your-first-request.md`
  2. `tutorials/make-your-first-api-call.md`
  3. `tutorials/build-your-first-plugin.md` (extracted from plugins.md)
  4. `tutorials/first-data-model.md` (extracted from data-modeling.md)

**Reference (significant gap)**
Currently only 2 reference documents. Users cannot look up configuration options, authentication parameters, deployment environment variables, or plugin APIs.

- Recommended new reference docs:
  1. `reference/configuration-options.md` (extracted from configuration.md)
  2. `reference/auth-api.md` (extracted from authentication.md)
  3. `reference/plugin-api.md` (extracted from plugins.md)
  4. `reference/schema.md` (extracted from data-modeling.md)

**Explanation (moderate gap)**
Currently 2 explanations. The architecture and data-modeling concepts are covered, but authentication concepts, API design philosophy, and plugin architecture are not.

- Recommended new explanations:
  1. `explanation/authentication-concepts.md` (extracted from authentication.md)
  2. `explanation/plugin-architecture.md` (extracted from plugins.md)
  3. `explanation/api-design-philosophy.md`

**How-to Guides (lowest gap)**
Currently 4 how-to guides. This is the best-covered quadrant, though there are still gaps for API integration patterns and plugin publishing.

- Recommended new how-to guides:
  1. `guides/how-to-paginate-results.md` (extracted from api-reference.md)
  2. `guides/how-to-publish-a-plugin.md` (extracted from plugins.md)

---

## Cross-Reference Audit

Documents with **no outgoing cross-references** to other quadrants:

| Document | Links to Tutorial | Links to How-to | Links to Reference | Links to Explanation |
|----------|:-----------------:|:---------------:|:------------------:|:--------------------:|
| `getting-started.md` | - | No | No | No |
| `authentication.md` | No | - | No | No |
| `api-reference.md` | No | No | - | No |
| `configuration.md` | No | - | No | No |
| `architecture.md` | No | No | No | - |
| `deployment.md` | No | - | No | No |
| `troubleshooting.md` | No | - | No | No |
| `data-modeling.md` | No | No | No | - |
| `cli-reference.md` | No | No | - | No |
| `plugins.md` | No | No | No | No |

**Finding:** Zero documents contain cross-references to other quadrants. This is a critical gap. Every document should link to related content in at least one other quadrant:

- Tutorials should link to: related how-to guides, explanation for deeper understanding
- How-to guides should link to: reference for detailed options, explanation for context
- Reference should link to: how-to guides for practical usage
- Explanation should link to: tutorials for hands-on learning, reference for technical details

---

## Priority Action Plan

### Immediate (Week 1) -- Highest Impact

1. **Split `plugins.md`** into four quadrant-specific documents. This single file is the worst offender and currently serves no audience effectively.
2. **Extract reference tables** from `authentication.md` and `configuration.md` into dedicated reference docs. These are the most commonly needed lookups and are buried inside how-to content.
3. **Add cross-references** to all 10 existing documents. This is low-effort, high-value work that immediately improves navigation.

### Short-term (Weeks 2-3) -- Fill Tutorial Gap

4. **Create `tutorials/authenticate-your-first-request.md`** -- authentication is a universal first barrier.
5. **Create `tutorials/make-your-first-api-call.md`** -- the API is the primary product surface.
6. **Extract tutorial content** from `data-modeling.md` into `tutorials/first-data-model.md`.

### Medium-term (Weeks 4-6) -- Fill Reference and Explanation Gaps

7. **Create missing reference docs** for deployment env vars and schema definitions.
8. **Create `explanation/authentication-concepts.md`** -- OAuth2/JWT concepts extracted and expanded.
9. **Create `explanation/api-design-philosophy.md`** -- why the API works the way it does.

### Ongoing

10. **Add `diataxis_type` frontmatter** to every document as it is created or updated.
11. **Enforce mode purity** in review -- any PR adding content to a doc should be checked against that doc's quadrant.
12. **Re-run this gap analysis quarterly** as documentation grows.

---

## Appendix: Recommended Directory Structure

```
docs/
  tutorials/
    getting-started.md
    authenticate-your-first-request.md
    make-your-first-api-call.md
    build-your-first-plugin.md
    first-data-model.md
  guides/
    how-to-configure-authentication.md
    how-to-configure-deployment.md
    how-to-deploy-to-production.md
    how-to-troubleshoot.md
    how-to-paginate-results.md
    how-to-publish-a-plugin.md
  reference/
    api-endpoints.md
    cli-commands.md
    configuration-options.md
    auth-api.md
    plugin-api.md
    schema.md
  explanation/
    architecture.md
    data-modeling.md
    authentication-concepts.md
    plugin-architecture.md
    api-design-philosophy.md
```

This structure physically separates quadrants, making it obvious when a document drifts outside its designated mode and making it easier for users to find content matching their current need.
