# Diátaxis Classification Compass

Complete decision tree for classifying documentation into the four Diátaxis quadrants.

## The Two Axes

### Axis 1: Action vs. Cognition

**Action** — The content involves practical steps: doing, making, building, configuring, deploying.
- Key signal: the reader is expected to perform operations
- Contains: commands, steps, procedures, code to execute

**Cognition** — The content involves understanding: concepts, theory, design, reasoning, context.
- Key signal: the reader is expected to absorb knowledge
- Contains: descriptions, explanations, comparisons, background

### Axis 2: Acquisition vs. Application

**Acquisition** — The user is in study mode: learning, exploring, building foundational knowledge.
- Key signal: the user does NOT yet have the skill/knowledge
- Context: onboarding, first contact, building understanding

**Application** — The user is in work mode: using existing knowledge to accomplish a goal.
- Key signal: the user already HAS baseline competence
- Context: solving a problem, looking up details, completing a task

## Decision Tree

```
Is the primary purpose of this content to guide DOING or KNOWING?

├── DOING (action-oriented)
│   │
│   ├── Is the reader LEARNING a new skill?
│   │   └── YES → TUTORIAL
│   │       "We guide you through building X to learn Y"
│   │
│   └── Is the reader ACCOMPLISHING a specific task?
│       └── YES → HOW-TO GUIDE
│           "Follow these steps to achieve X"
│
└── KNOWING (cognition-oriented)
    │
    ├── Is the reader LOOKING UP technical details?
    │   └── YES → REFERENCE
    │       "X accepts parameters A, B, C and returns D"
    │
    └── Is the reader UNDERSTANDING concepts/context?
        └── YES → EXPLANATION
            "The reason X works this way is because..."
```

## Classification by Signal Words

| Signal | Quadrant | Example |
|--------|----------|---------|
| "In this tutorial, we will..." | Tutorial | Learning introduction |
| "Let's build..." / "Follow along..." | Tutorial | Guided experience |
| "You will learn..." | Tutorial | Learning goal |
| "How to..." / "Steps to..." | How-to | Task heading |
| "If you need to..." | How-to | Conditional instruction |
| "This guide shows you how to..." | How-to | Problem statement |
| "`function(param)`" / "Returns:" | Reference | API description |
| "Options:" / "Parameters:" | Reference | Configuration listing |
| "Type: string \| Default: null" | Reference | Specification |
| "The reason for..." / "Why..." | Explanation | Conceptual context |
| "Historically, this..." | Explanation | Background |
| "Compared to X, Y is..." | Explanation | Analysis |

## Common Misclassifications

### Tutorial mistaken for How-to
- **Test:** Does the reader already know what they want to do?
  - No → Tutorial (they're learning)
  - Yes → How-to (they have a goal)

### How-to mistaken for Reference
- **Test:** Is the content organized by user task or by product structure?
  - By task → How-to
  - By product/API → Reference

### Reference mistaken for Explanation
- **Test:** Does the content describe WHAT exists or WHY it exists?
  - What → Reference
  - Why → Explanation

### Explanation mistaken for Tutorial
- **Test:** Is the reader expected to perform actions or absorb concepts?
  - Perform actions → Tutorial
  - Absorb concepts → Explanation

## Detecting Mode Mixing

Scan for these patterns to identify quadrant contamination:

| In this quadrant... | Watch for... | It's actually... |
|---------------------|--------------|------------------|
| Tutorial | Extended parameter lists | Reference leaking in |
| Tutorial | "The reason this works is..." | Explanation leaking in |
| How-to | "Let's first understand..." | Explanation leaking in |
| How-to | Comprehensive option tables | Reference leaking in |
| Reference | "First, do X, then Y..." | How-to leaking in |
| Reference | "The design philosophy..." | Explanation leaking in |
| Explanation | Step-by-step procedures | Tutorial/How-to leaking in |
| Explanation | API signatures and types | Reference leaking in |

**Resolution:** Extract contaminating content into its proper quadrant document and replace with a cross-reference link.
