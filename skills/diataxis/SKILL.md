---
name: diataxis
description: This skill should be used when the user asks to "apply diataxis", "classify documentation", "review docs with diataxis", "create diataxis documentation", "documentation gap analysis", "check documentation quadrants", "organize docs by type", or needs guidance on structuring documentation according to the Diataxis framework (tutorials, how-to guides, reference, explanation).
version: 0.1.0
---

# Diátaxis Documentation Framework

<!-- BEGIN MNEMONIC PROTOCOL -->
## Memory Operations

You have PERSISTENT MEMORY across sessions.

BEFORE starting any task:
```bash
if [ -d ~/.claude/mnemonic ]; then
    rg -i "{diataxis}" ~/.claude/mnemonic/ --glob "*.memory.md" -l | head -5
fi
```
If results exist, READ the most relevant and apply that context.

AFTER completing work, if you discovered:
- A decision → capture to _semantic/decisions
- A pattern → capture to _procedural/patterns
- A learning → capture to _semantic/knowledge
- A blocker → capture to _episodic/blockers
<!-- END MNEMONIC PROTOCOL -->

---

Comprehensive guidance for classifying, creating, reviewing, and maintaining documentation according to the Diátaxis framework. Diátaxis identifies four distinct documentation modes, each serving a different user need. Documentation quality depends on keeping these modes separate and well-executed.

## The Diátaxis Compass

Two fundamental axes classify all documentation:

|                | **Acquisition** (study/learning) | **Application** (work/doing) |
|----------------|----------------------------------|------------------------------|
| **Action**     | Tutorial                         | How-to Guide                 |
| **Cognition**  | Explanation                      | Reference                    |

**Diagnostic questions:**
1. Does this content involve **practical steps** (action) or **theoretical knowledge** (cognition)?
2. Is the user **learning something new** (acquisition) or **applying existing skill** (application)?

The answers place content in exactly one quadrant.

---

## The Four Documentation Modes

### 1. Tutorials (Learning-oriented)

**Purpose:** Guide a learner through a series of steps to acquire skills and understanding.

**Defining characteristics:**
- The teacher takes full responsibility for the learner's success
- Learning happens through doing, not through explanation
- The learner follows a carefully designed path to a meaningful result

**What a tutorial MUST do:**
- Show the destination upfront — what the learner will accomplish
- Deliver visible results at every step — each action produces comprehensible output
- Maintain narrative expectations — "You will notice that...", "The output should look like..."
- Point out observable details — direct the learner's attention to important cues
- Target the "feeling of doing" — help learners discover rhythm and satisfaction
- Encourage repetition — design steps that reward being repeated
- Aspire to perfect reliability — learners must get the promised results every time

**What a tutorial MUST NOT do:**
- Explain concepts in depth — link to explanation docs instead
- Present options or alternatives — keep the path singular and focused
- Assume prior knowledge — start from the beginning
- Digress into reference material — stay on the learning path
- Teach — enable learning through guided experience instead

**Language and tone:**
- Use inclusive first-person plural: "In this tutorial, we will..."
- Be direct: "First, do x. Now, do y. Now that you have done y, do z."
- Set clear expectations: "The output should look something like..."
- Acknowledge accomplishment: describe what the learner has built

**Structure template:**
```markdown
# Tutorial: [What You Will Build/Learn]

## What you'll learn
[Brief description of skills acquired]

## Prerequisites
[Minimal list — keep this short]

## Steps

### Step 1: [Action]
[Direct instruction]
[Expected result]

### Step 2: [Action]
[Direct instruction]
[Expected result]

...

## What you've accomplished
[Summary of what was built and skills gained]

## Next steps
[Links to related how-to guides and explanation]
```

**Frontmatter:**
```yaml
---
diataxis_type: tutorial
diataxis_learning_goals:
  - [skill or concept acquired]
---
```

---

### 2. How-to Guides (Task-oriented)

**Purpose:** Direct a competent user through the steps to solve a specific, real-world problem.

**Defining characteristics:**
- Goal-oriented — the user knows what they want to achieve
- Addresses a real-world problem, not a learning exercise
- Assumes baseline competence — the reader can follow directions
- Contains only action and instruction — no digression

**What a how-to guide MUST do:**
- State the problem or goal clearly in the title
- Sequence steps logically — follow the user's cognitive workflow
- Address real-world complexity — problems fork, overlap, require judgment
- Write from the user's perspective, not the machinery's perspective
- Remain focused — start and end at reasonable, meaningful places

**What a how-to guide MUST NOT do:**
- Teach or explain — the user already has baseline competence
- Include exhaustive reference — link to reference docs instead
- Document every option — focus on what solves the problem
- Duplicate tutorial content — these serve different users at different stages

**Language and tone:**
- Use conditional imperatives: "If you want x, do y"
- State the problem: "This guide shows you how to..."
- Defer comprehensive options: "Refer to the reference guide for full options"

**Naming convention:**
- Good: "How to integrate application performance monitoring"
- Bad: "Application performance monitoring" (unclear intent)

**Structure template:**
```markdown
# How to [Achieve Specific Goal]

## Overview
[One sentence: what this guide helps you accomplish]

## Prerequisites
[What the reader needs before starting]

## Steps

### 1. [First action]
[Instructions with context for decisions]

### 2. [Second action]
[Instructions]

...

## Verification
[How to confirm success]

## Troubleshooting
[Common issues and solutions — optional but valuable]

## Related
- [Link to reference docs for detailed options]
- [Link to explanation for deeper understanding]
```

**Frontmatter:**
```yaml
---
diataxis_type: how-to
diataxis_goal: [what the user accomplishes]
---
```

---

### 3. Reference (Information-oriented)

**Purpose:** Provide authoritative, accurate technical descriptions of the machinery and how to operate it.

**Defining characteristics:**
- Structured by the product, not by user tasks
- Austere, neutral, factual — no opinion, no instruction
- Consulted during work, not read end-to-end
- Must mirror the structure of the product it describes

**What reference documentation MUST do:**
- Describe what exists — functions, classes, APIs, CLI options, config keys
- Adopt standard, consistent patterns — users must find info where they expect it
- Respect the product's own structure — align docs with code architecture
- Provide examples — concise illustrations of usage, not tutorials
- Be accurate and complete — "truth and certainty"
- Use prescriptive language where needed: "You must use a. Never use d."

**What reference documentation MUST NOT do:**
- Instruct — link to how-to guides instead
- Explain concepts — link to explanation docs instead
- Include opinions, marketing claims, or interpretation
- Rely solely on auto-generated docs — supplement with authored content

**Language and tone:**
- State facts directly
- List options, parameters, return types, error codes
- Include warnings where appropriate
- Maintain neutral, objective presentation

**Structure template:**
```markdown
# [Component/API/Module Name]

## Overview
[One-line description of what this is]

## [Section matching product structure]

### `function_name(param1, param2)`

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|
| param1 | string | Yes | Description |
| param2 | int | No | Description (default: 0) |

**Returns:** `ReturnType` — description

**Raises/Errors:**
- `ErrorType` — when condition

**Example:**
\```language
brief_usage_example()
\```

## See also
- [Related reference pages]
- [How-to guides using this component]
```

**Frontmatter:**
```yaml
---
diataxis_type: reference
diataxis_describes: [component, API, or system being documented]
---
```

---

### 4. Explanation (Understanding-oriented)

**Purpose:** Deepen understanding through discursive treatment of a topic, providing context, history, and interconnected knowledge.

**Defining characteristics:**
- Takes a higher, wider view than the other three modes
- Not consulted while actively working — read reflectively
- Addresses "why" questions and conceptual understanding
- Characterized by distance from immediate practical concerns

**What explanation documentation MUST do:**
- Make connections — link topics together, even outside the immediate subject
- Provide context — include background, design decisions, historical reasons, constraints
- Structure around topics — cover the bigger picture, history, choices, alternatives
- Embrace perspective — consider alternatives, counter-examples, multiple approaches
- Stay tightly bounded — resist absorbing other modes into the discussion

**What explanation documentation MUST NOT do:**
- Include step-by-step instructions — that's a tutorial or how-to
- Provide machinery descriptions — that's reference
- Substitute for practical guides — understanding is not action
- Drift without boundaries — define scope clearly

**Language and tone:**
- Use causal reasoning: "The reason for x is because historically, y..."
- Offer judgments: share informed perspective
- Employ analogies: "An x in system y is analogous to a w in system z"
- Weigh alternatives: "Some users prefer w (because z). This can be a good approach, but..."

**Structure template:**
```markdown
# [Topic]: [Understanding/Context Aspect]

## Background
[Historical context, design decisions, constraints]

## How [topic] works
[Conceptual description — not step-by-step]

## Why [approach] was chosen
[Design rationale, alternatives considered, trade-offs]

## Comparison with alternatives
[Other approaches, their strengths and weaknesses]

## Implications
[What this means for users, architecture, future decisions]

## Further reading
- [Links to tutorials for hands-on learning]
- [Links to reference for technical details]
- [Links to related explanation topics]
```

**Frontmatter:**
```yaml
---
diataxis_type: explanation
diataxis_topic: [conceptual subject being discussed]
---
```

---

## Operations

### Classification

To classify a document into its Diátaxis quadrant:

1. **Read the document** and identify its primary intent
2. **Apply the compass questions:**
   - Action or cognition?
   - Acquisition or application?
3. **Check for quadrant mixing** — flag sections where the mode shifts:
   - Tutorial that drifts into reference → extract reference to its own doc
   - How-to that starts explaining "why" → extract explanation to its own doc
   - Reference that includes step-by-step → extract to a how-to guide
   - Explanation that lists parameters → extract to reference
4. **Assign the frontmatter** `diataxis_type` based on the dominant mode
5. **Recommend splits** if a document significantly mixes modes

### Review (Diátaxis Quality Criteria)

When reviewing documentation against Diátaxis, evaluate:

**Per-quadrant criteria:**

| Criterion | Tutorial | How-to | Reference | Explanation |
|-----------|----------|--------|-----------|-------------|
| Purpose clarity | Learning goals stated | Goal/problem stated | Component identified | Topic bounded |
| Mode purity | No reference dumps | No teaching | No instructions | No step-by-step |
| Audience fit | Beginner-appropriate | Competent user | Working practitioner | Curious learner |
| Structure | Step-by-step path | Logical sequence | Mirrors product | Topic-driven |
| Tone | Guided, encouraging | Direct, practical | Austere, factual | Discursive, reflective |
| Completeness | Achieves stated goal | Solves stated problem | Covers all elements | Provides full context |
| Cross-references | Links to how-to, explanation | Links to reference | Links to how-to | Links to reference, tutorial |

**Strictness levels:**

- **advisory** — Suggestions only. Note potential Diátaxis improvements without flagging as issues.
- **standard** — Flag mode mixing and missing quadrant metadata as issues in the review report.
- **strict** — Treat Diátaxis violations as errors: mode mixing is critical, missing frontmatter is major, poor cross-references are minor.

### Creation

When creating new documentation:

1. **Determine the quadrant** — ask: "What is the user trying to do with this document?"
   - Learn something new → Tutorial
   - Accomplish a specific task → How-to Guide
   - Look up technical details → Reference
   - Understand concepts and context → Explanation
2. **Apply the quadrant's structure template** (see above)
3. **Set the frontmatter** with `diataxis_type`
4. **Write in the quadrant's tone and style**
5. **Add cross-references** to documents in other quadrants
6. **Validate** against the per-quadrant quality criteria

### Update

When updating existing documentation:

1. **Check the current quadrant** — read `diataxis_type` from frontmatter (or classify if missing)
2. **Evaluate the proposed changes** against the quadrant's rules
3. **Flag quadrant drift** — if the update would shift the document toward another mode:
   - Warn: "This addition is [explanation/reference/etc.] content being added to a [tutorial/how-to/etc.]"
   - Suggest: extract drifting content into its own document in the correct quadrant
4. **Maintain cross-references** — update links between related documents

### Gap Analysis

Analyze a project's documentation to identify missing quadrants:

1. **Inventory** all documentation files
2. **Classify** each into its Diátaxis quadrant
3. **Map** documentation coverage per feature/component:

```markdown
## Diátaxis Coverage Matrix

| Feature/Component | Tutorial | How-to | Reference | Explanation |
|-------------------|----------|--------|-----------|-------------|
| Authentication    | -        | Yes    | Yes       | -           |
| API Endpoints     | -        | -      | Yes       | -           |
| Configuration     | -        | Yes    | -         | -           |
| Architecture      | -        | -      | -         | Yes         |

### Gaps Identified
- Authentication: Missing tutorial (getting started) and explanation (auth concepts)
- API Endpoints: Missing how-to guides and tutorials
- Configuration: Missing reference (full config options list)
```

4. **Prioritize gaps** by user impact:
   - No tutorials → new users cannot get started
   - No how-to guides → users cannot solve problems
   - No reference → users cannot look up details
   - No explanation → users lack conceptual understanding

### Cross-reference Audit

Verify documentation links correctly between quadrants:

- Tutorials should link to: related how-to guides, explanation for deeper understanding
- How-to guides should link to: reference for detailed options, explanation for context
- Reference should link to: how-to guides for practical usage
- Explanation should link to: tutorials for hands-on learning, reference for technical details

Flag documents with no outgoing cross-references to other quadrants.

---

## Configuration

The Diátaxis skill reads configuration from `.claude/documentation-review.local.md` frontmatter:

```yaml
diataxis:
  enabled: true                    # Default: true — Diátaxis is enabled for all projects
  strictness: strict               # Default: strict — advisory | standard | strict
  require_frontmatter: true        # Require diataxis_type in doc frontmatter
  doc_paths: []                    # Paths to scan (empty = use plugin doc_paths)
  quadrant_directories:            # Optional: organize docs by quadrant directory
    tutorials: docs/tutorials/
    how-to: docs/guides/
    reference: docs/reference/
    explanation: docs/explanation/
```

**Defaults (no config required):** Diátaxis is enabled at `strict` strictness for all projects using the documentation-review plugin. Projects can override by setting `diataxis.enabled: false` or adjusting `strictness` in their `.claude/documentation-review.local.md`.

When `diataxis.doc_paths` is empty, the skill falls back to the plugin's top-level `doc_paths` configuration. When that is also absent, require the user to specify paths.

---

## Integration with Existing Plugin

This skill augments the existing documentation-review commands:

- **`/doc-review`** — When Diátaxis is enabled, adds a "Diátaxis Alignment" dimension to the review report with quadrant classification, mode purity score, and cross-reference completeness
- **`/doc-create`** — When Diátaxis is enabled, asks which quadrant the new document belongs to and applies the appropriate structure template, tone, and frontmatter
- **`/doc-update`** — When Diátaxis is enabled, checks proposed changes for quadrant drift and warns before introducing mixed-mode content
- **`/doc-cleanup`** — When Diátaxis is enabled, includes gap analysis showing missing quadrants per feature/component

## Additional Resources

### Reference Files

For detailed guidance, consult:
- **`references/compass.md`** — Full Diátaxis classification decision tree with examples
- **`references/quality-criteria.md`** — Detailed per-quadrant quality criteria and scoring rubrics

### Example Files

Working examples in `examples/`:
- **`tutorial-example.md`** — Well-structured tutorial following Diátaxis principles
- **`howto-example.md`** — Well-structured how-to guide following Diátaxis principles
- **`reference-example.md`** — Well-structured reference following Diátaxis principles
- **`explanation-example.md`** — Well-structured explanation following Diátaxis principles
