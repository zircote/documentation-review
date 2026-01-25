---
description: Generate new documentation from codebase analysis
argument-hint: "<type: readme|api|template>"
allowed-tools: Read, Write, Glob, Grep, Bash(git:*), Bash(rg:*), Bash(mkdir:*), Bash(cat:*), Bash(uuidgen:*)
---

Generate new documentation based on codebase analysis.

## Documentation Type

$IF($1,
  Generate: $1 documentation,
  Ask the user which type of documentation to create:
  - readme: Generate README.md from project analysis
  - api: Generate API documentation from code/specs
  - template: Create documentation from standard templates
)

## Pre-Generation: Check Memory

Before generating documentation, search mnemonic for relevant context:

```bash
# Check for existing documentation patterns
rg -i "documentation|readme|api-doc" ~/.claude/mnemonic/ --glob "*.memory.md" -l | head -5

# Check for project-specific documentation decisions
rg -i "doc|writing|style" ~/.claude/mnemonic/*/decisions/ --glob "*.memory.md" -l | head -5
```

Apply any found patterns or preferences to the generation.

## Generation Workflows

### README Generation (type: readme)

1. **Analyze Project Structure**
   - Find package.json, pyproject.toml, go.mod, Cargo.toml
   - Identify primary language and framework
   - List main source directories

2. **Extract Project Information**
   - Project name and description from manifest
   - Dependencies and their purposes
   - Available scripts/commands
   - Entry points and exports

3. **Generate Sections**
   - Title and description
   - Features (from code analysis)
   - Installation (from package manager)
   - Quick start (from examples or main entry)
   - Configuration (from config files)
   - API overview (from exports)
   - Contributing (standard template)
   - License

4. **Write README.md**
   Use Write tool to create the file.

### API Documentation (type: api)

1. **Detect API Specification**
   Check for existing specs:
   - openapi.yaml, openapi.json, swagger.yaml
   - asyncapi.yaml, asyncapi.json

   If no spec found, analyze source code.

2. **From OpenAPI/Swagger**
   - Parse specification file
   - Extract endpoints, parameters, responses
   - Generate markdown documentation
   - Include code examples

3. **From Source Code**
   - Find route definitions
   - Extract endpoint handlers
   - Document request/response types
   - Generate example payloads

4. **Write API Documentation**
   Create docs/api.md or docs/api/ directory.

### Template-Based (type: template)

1. **List Available Templates**
   Check ${CLAUDE_PLUGIN_ROOT}/templates/ for:
   - contributing.md
   - architecture.md
   - deployment.md
   - troubleshooting.md

2. **Select Template**
   If no $2 argument, ask user which template.

3. **Customize Template**
   - Replace placeholders with project info
   - Add project-specific sections
   - Update examples for project context

4. **Write Documentation**
   Create file in appropriate location.

## Quality Assurance

After generation:

1. **Validate Content**
   - Check all code examples are syntactically correct
   - Verify links are valid
   - Ensure no placeholder text remains

2. **Apply Standards**
   Use documentation-standards skill to ensure:
   - Proper heading hierarchy
   - Consistent formatting
   - Complete sections

3. **Preview for User**
   Show generated content summary:
   - File path
   - Sections created
   - Word count
   - Any warnings

## Output

Report what was created:

```markdown
## Documentation Generated

**Type:** [readme|api|template]
**File:** [path]
**Sections:** [list of sections]

### Summary
[Brief description of what was generated]

### Next Steps
1. Review generated content
2. Customize as needed
3. Run /doc-review to validate
```

## Post-Creation: Capture to Mnemonic

After successfully creating documentation, capture the pattern to mnemonic:

```bash
UUID=$(uuidgen | tr '[:upper:]' '[:lower:]')
DATE=$(date -u +"%Y-%m-%dT%H:%M:%SZ")
SLUG="doc-creation-[type]"

mkdir -p ~/.claude/mnemonic/default/patterns/user

cat > ~/.claude/mnemonic/default/patterns/user/${UUID}-${SLUG}.memory.md << MEMORY
---
id: ${UUID}
type: procedural
namespace: patterns/user
created: ${DATE}
title: "Documentation Creation Pattern: [type]"
tags:
  - documentation
  - [type]
  - generation
---

# Documentation Creation Pattern

## Level 1: Quick Answer
Created [type] documentation with [sections] structure.

## Level 2: Context
- File: [path]
- Sections: [list of sections created]
- Style: [any style decisions made]

## Level 3: Full Detail
[Capture structure, sections used, and any project-specific adaptations]
MEMORY
```
