---
description: Generate new documentation from codebase analysis
argument-hint: "<type: readme|api|template>"
allowed-tools: Read, Write, Glob, Grep, Bash(git:*), Bash(rg:*), Bash(mkdir:*), Bash(cat:*), Bash(uuidgen:*)
---

<!-- BEGIN MNEMONIC PROTOCOL -->
## Memory Operations

You have PERSISTENT MEMORY across sessions.

BEFORE starting any task:
```bash
if [ -d ~/.claude/mnemonic ]; then
    rg -i "{documentation_type}" ~/.claude/mnemonic/ --glob "*.memory.md" -l | head -5
fi
```
If results exist, READ the most relevant and apply that context.

AFTER completing work, if you discovered:
- A decision → capture to _semantic/decisions
- A pattern → capture to _procedural/patterns
- A learning → capture to _semantic/knowledge
- A blocker → capture to _episodic/blockers
<!-- END MNEMONIC PROTOCOL -->

Generate new documentation based on codebase analysis.

## Documentation Type

$IF($1,
  Generate: $1 documentation,
  Ask the user which type of documentation to create:
  - readme: Generate README.md from project analysis
  - api: Generate API documentation from code/specs
  - template: Create documentation from standard templates
)

## Diátaxis Quadrant Selection (enabled by default; disable with `diataxis.enabled: false`)

Before generating content, determine the Diátaxis quadrant:

1. **Check if the documentation type maps to a quadrant:**
   - readme → typically mixed (overview), flag for later splitting
   - api → Reference quadrant
   - template → depends on template type

2. **Ask the user which quadrant the document serves:**
   - Tutorial: learning-oriented, guided experience
   - How-to: task-oriented, solve a specific problem
   - Reference: information-oriented, technical descriptions
   - Explanation: understanding-oriented, context and background

3. **Apply the quadrant's structure template** from the `diataxis` skill
4. **Set `diataxis_type` in frontmatter** of the generated document
5. **Use the quadrant's tone and style** guidelines during generation

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

   When Diátaxis is enabled, also validate:
   - Content stays within the chosen quadrant (mode purity)
   - Cross-references to other quadrants are included
   - Frontmatter includes `diataxis_type`
   Use the diataxis skill for quadrant templates and quality criteria.

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

