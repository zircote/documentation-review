---
title: "Understanding the Diataxis Framework"
diataxis_type: explanation
---

# Understanding the Diataxis Framework

Documentation fails not because writers lack knowledge, but because they mix fundamentally different types of information into a single page. A tutorial that suddenly becomes a reference table, a how-to guide that pauses to teach theory, a reference page that tries to motivate its reader — these are symptoms of a deeper problem. The Diataxis framework exists to solve it.

## Origins and Purpose

Diataxis was developed by Daniele Procida, drawing on years of experience maintaining documentation for Django and other open-source projects. The name comes from the Greek *dia* (across) and *taxis* (arrangement) — literally, "a systematic arrangement across categories."

The framework emerged from a practical observation: documentation that tried to do everything at once served no audience well. Procida identified that what people need from documentation varies based on two independent factors — what they're trying to accomplish and what stage of learning they're in. This insight leads to four distinct documentation modes, each serving a different cognitive need.

## The Two Axes

Diataxis organizes documentation along two axes that reflect how people interact with knowledge:

**Action vs. Cognition** — Is the reader trying to *do something* or trying to *understand something*? A developer debugging a deployment issue needs actionable steps, not a lecture on networking theory. Conversely, a developer choosing between architectural approaches needs conceptual grounding, not a recipe.

**Acquisition vs. Application** — Is the reader *learning something new* or *applying existing knowledge*? A newcomer following their first setup has different needs than an experienced user configuring an advanced feature they already understand conceptually.

These two axes create four quadrants, and each quadrant corresponds to a documentation type with its own internal logic, tone, and structure.

## The Four Quadrants

### Tutorials: Learning by Doing

Tutorials sit at the intersection of action and acquisition. They serve readers who are new to a topic and need guided, hands-on experience. The reason tutorials exist as a distinct mode is that learning requires a carefully controlled environment — the writer must make deliberate choices about what to include and what to defer, ensuring the learner builds confidence through incremental success.

A tutorial is not a feature tour. It is a lesson, and the writer is the teacher. The reader trusts that every step is necessary and that nothing will break. This is why tutorials are the hardest documentation to write well — they demand empathy for someone who does not yet have the context you take for granted.

### How-to Guides: Solving Real Problems

How-to guides serve readers who already understand the basics and need to accomplish a specific, real-world goal. They sit at the intersection of action and application. Where a tutorial says "let me teach you," a how-to guide says "here's how to get this done."

The distinction matters because the reader's cognitive state is fundamentally different. A how-to reader has context. They know what they want. They do not need motivation or explanation — they need clear, reliable steps that respect their existing knowledge.

### Reference: Looking Up Facts

Reference documentation serves readers who need accurate, complete, and consistently structured information about the system. It sits at the intersection of cognition and application. Reference material is austere by design — its job is to describe the machinery, not to teach or persuade.

Reference documentation is shaped by the code itself rather than by user needs. An API reference mirrors the API's structure. A configuration reference mirrors the configuration schema. This alignment is deliberate: when a reader looks something up, they already know *where* to look, and the documentation should match their mental model of the system's structure.

### Explanation: Understanding Context

Explanation — where this document itself lives — serves readers who want to deepen their understanding. It sits at the intersection of cognition and acquisition. Explanation addresses "why" questions: why does this design exist? Why was this approach chosen over alternatives? What are the trade-offs?

Explanation is the most discursive mode. It can offer opinions, draw analogies, provide historical context, and weigh alternatives. Its purpose is to build a conceptual framework that makes the other three documentation types more useful. A reader who understands *why* the system works a certain way will navigate tutorials, how-to guides, and reference material more effectively.

## Why Separating Modes Matters

The reason Diataxis insists on separating documentation modes is rooted in cognitive science. When a reader approaches documentation, they are in a particular mental state — learning, doing, or looking up — and content that violates that state creates friction.

Consider a tutorial that suddenly includes a complete reference table of all configuration options. The learner, who was building confidence through guided steps, is now overwhelmed by information they cannot yet contextualize. Or consider a reference page that begins with a motivational paragraph about why the feature is useful. The experienced developer looking up a parameter's default value must wade through content that wastes their time.

Mode mixing does not just annoy readers — it undermines the documentation's core purpose. Mixed-mode documents serve no audience's needs fully while appearing to serve everyone's. The result is documentation that looks comprehensive but fails in practice.

## How the Plugin Enforces Diataxis

The documentation-review plugin translates the Diataxis framework from theory into enforceable practice through several mechanisms.

### Frontmatter Classification

Every document must declare its Diataxis type via a `diataxis_type` field in its frontmatter. This seemingly simple requirement has a profound effect: it forces the writer to commit to a single documentation mode *before* writing. The act of choosing — tutorial, how-to, reference, or explanation — primes the writer to stay within that mode's conventions.

### Mode Purity Scoring

The plugin analyzes document content against the declared type and produces a purity score. A tutorial that drifts into reference-style tables or an explanation that includes step-by-step procedures will receive a lower score. This addresses the most common failure mode in documentation: unconscious mode mixing that happens when a writer tries to be "complete."

### Cross-Reference Auditing

Diataxis does not mean documentation types should be isolated. On the contrary, they should reference each other extensively. A tutorial should link to reference material for readers who want details. A how-to guide should link to explanations for readers who want context. The plugin audits these cross-references to ensure documents connect properly across types.

### Gap Analysis

The plugin performs gap analysis using a coverage matrix that maps features and components against documentation types. If a feature has reference documentation but no tutorial, or a how-to guide but no explanation, the gap analysis surfaces this. The goal is balanced coverage — every significant feature should eventually be documented across all four modes.

### Strictness Levels

The plugin offers three strictness levels — advisory, standard, and strict — reflecting the reality that organizations adopt Diataxis incrementally. Advisory mode provides suggestions without blocking. Standard mode flags issues that should be addressed. Strict mode treats violations as errors.

The relationship between strictness and organizational maturity is deliberate. A team just beginning to adopt structured documentation benefits from advisory mode, which educates without creating friction. As documentation culture matures and conventions become second nature, stricter enforcement prevents regression.

## Common Pitfalls

Even with framework awareness, certain patterns of mode mixing recur across projects:

**Tutorials that become reference dumps.** A writer, wanting to be thorough, includes every option for a command or every field in a configuration file within a tutorial. The learner, who needs exactly three options to complete the exercise, drowns in information.

**How-to guides that teach.** A guide titled "How to Configure Authentication" opens with three paragraphs explaining what authentication is and why it matters. The reader who searched for this guide already knows what authentication is — they need the steps.

**Reference that instructs.** A configuration reference includes notes like "you should set this to true in production." Reference material describes what exists; guidance about what to *do* belongs in how-to guides.

**Explanation that prescribes.** An explanation about caching strategies includes specific commands to run. The conceptual discussion is valuable, but the actionable steps should live in a linked how-to guide.

Recognizing these patterns is the first step toward preventing them. The documentation-review plugin automates this recognition, but understanding *why* each pattern is problematic makes the automated feedback more actionable.

## Further Reading

- [Enforce Diataxis](../guides/enforce-diataxis.md) for practical steps on configuring and applying Diataxis enforcement
- [Commands Reference](../reference/commands.md) for `/doc-diataxis` command details and options
- [Skills Reference](../reference/skills.md) for the diataxis skill's capabilities
- [Getting Started](../tutorials/getting-started.md) for a hands-on introduction to the plugin
- [Why Documentation Quality Matters](../explanation/documentation-quality.md) for the companion discussion on quality, drift, and automation
