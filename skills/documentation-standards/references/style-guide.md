# Documentation Style Guide

Comprehensive style rules for technical documentation.

## Language and Grammar

### Sentence Structure
- Keep sentences under 25 words when possible
- One idea per sentence
- Use parallel construction in lists
- Avoid nested clauses

### Word Choice
- Prefer simple words over complex: "use" not "utilize"
- Be specific: "click the Save button" not "click the button"
- Avoid filler words: "very", "really", "basically"
- Use consistent terminology throughout

### Tense
- **Procedures:** Present tense ("Click the button")
- **Results:** Present tense ("The dialog opens")
- **Prerequisites:** Present perfect ("You have installed Node.js")
- **Release notes:** Past tense ("Fixed the bug")

### Person
- **Instructions:** Second person ("You can configure...")
- **API docs:** Third person ("The function returns...")
- **Concepts:** Third person ("The system processes...")

## Formatting Standards

### Capitalization
- Sentence case for headings: "Getting started with the API"
- Title case only for proper nouns
- ALL CAPS only for acronyms
- Match UI labels exactly

### Punctuation
- Use serial (Oxford) comma: "A, B, and C"
- One space after periods
- No periods in headings
- Use em-dashes (—) sparingly

### Numbers
- Spell out one through nine
- Use numerals for 10 and above
- Always use numerals with units: "5 GB"
- Use numerals in technical contexts: "port 8080"

## Code Documentation

### Inline Code
Use backticks for:
- File names: `package.json`
- Directory names: `src/components/`
- Commands: `npm run build`
- Variable/function names: `getUserById()`
- Configuration values: `"production"`
- Short code: `return null`

### Code Blocks
- Always specify language identifier
- Keep examples concise (under 30 lines)
- Include comments for complex logic
- Show expected output when helpful
- Use realistic but simple examples

### Command Examples
```bash
# Good: Shows expected output
npm install express
# Output: added 57 packages

# Good: Explains flags
npm install --save-dev jest  # Install as dev dependency
```

## Visual Elements

### Screenshots
- Crop to relevant area
- Highlight important elements
- Add captions describing the image
- Update when UI changes
- Provide alt text for accessibility

### Diagrams
- Use for complex relationships
- Keep simple and focused
- Include legend when needed
- Use consistent visual style
- Prefer text descriptions when possible

### Tables
- Use for comparing multiple items
- Keep columns to a minimum (3-5)
- Align numbers to the right
- Use consistent formatting
- Include header row

## Document Types

### README Files

**Required sections:**
1. Project name and description (1-2 sentences)
2. Key features (bullet list)
3. Installation instructions
4. Basic usage example
5. License

**Recommended sections:**
- Prerequisites
- Configuration
- API overview
- Contributing
- Changelog link

### API Documentation

**For each endpoint/function:**
1. Brief description (one sentence)
2. Signature/syntax
3. Parameters table
4. Return value
5. Example usage
6. Error conditions

### Tutorials

**Structure:**
1. Goal statement (what reader will accomplish)
2. Prerequisites (explicit list)
3. Numbered steps (one action each)
4. Verification (how to confirm success)
5. Troubleshooting (common issues)
6. Next steps (related tutorials)

### Reference Documentation

**Organize by:**
- Alphabetical (for large APIs)
- Functional grouping (by feature area)
- Workflow order (for procedures)

**Include for each item:**
- Name and type
- Description
- Parameters/options
- Examples
- Related items

## Accessibility Guidelines

### Headings
- Use proper hierarchy (H1 → H2 → H3)
- Make headings descriptive
- Don't skip levels

### Images
- Provide meaningful alt text
- Describe what the image shows
- Don't rely on images alone

### Links
- Use descriptive link text
- Avoid "click here" or "read more"
- Indicate external links

### Color
- Don't use color as sole indicator
- Ensure sufficient contrast
- Test with color blindness simulators

## Quality Metrics

### Readability
- Target Flesch-Kincaid grade 8-10
- Use readability tools to check
- Simplify complex sentences

### Completeness
- All features documented
- Prerequisites clearly stated
- Examples for all key operations
- Error handling documented

### Accuracy
- Technical details verified
- Examples tested and working
- Version numbers current
- Links validated

### Maintainability
- Modular structure (easy to update)
- Clear ownership
- Review schedule established
- Version history maintained
