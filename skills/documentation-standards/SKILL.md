---
name: Documentation Standards
description: This skill should be used when the user asks to "review documentation", "improve docs quality", "check markdown formatting", "fix documentation structure", "write better docs", "documentation best practices", or needs guidance on documentation quality, structure, formatting, or technical writing standards.
version: 0.1.0
---

# Documentation Standards

Guidance for creating, reviewing, and improving technical documentation with focus on quality, clarity, and maintainability.

## Core Quality Principles

### Clarity
- Write for the reader's level of expertise
- Define technical terms on first use
- Use active voice and direct language
- Avoid jargon unless necessary for the audience

### Completeness
- Cover all essential topics for the document's purpose
- Include prerequisites and requirements
- Provide working examples for technical concepts
- Document edge cases and limitations

### Accuracy
- Verify all technical details against current implementation
- Test code examples before including them
- Update documentation when code changes
- Remove or mark deprecated content clearly

### Consistency
- Follow established style guides (project or industry standard)
- Use consistent terminology throughout
- Maintain uniform formatting and structure
- Apply consistent code example styles

## Document Structure

### Standard Sections

**README files:**
1. Title and brief description
2. Features/highlights
3. Installation/setup
4. Quick start/usage
5. Configuration options
6. API reference (or link)
7. Contributing guidelines
8. License

**Technical guides:**
1. Overview/introduction
2. Prerequisites
3. Step-by-step instructions
4. Examples
5. Troubleshooting
6. Related resources

**API documentation:**
1. Endpoint/function description
2. Parameters (required/optional)
3. Request/response formats
4. Code examples
5. Error responses
6. Rate limits/constraints

### Heading Hierarchy

Follow semantic heading structure:
- H1: Document title (one per document)
- H2: Major sections
- H3: Subsections
- H4: Minor subsections (use sparingly)
- Avoid H5/H6 (indicates need for restructuring)

## Markdown Best Practices

### Code Blocks

Always specify language for syntax highlighting:

````markdown
```python
def example():
    return "highlighted"
```
````

Use inline code for:
- File names: `config.yaml`
- Commands: `npm install`
- Variable names: `user_id`
- Short code references: `return True`

### Links

**Internal links:**
- Use relative paths: `[Setup](./setup.md)`
- Verify links exist and are accurate
- Use descriptive link text (not "click here")

**External links:**
- Include for authoritative sources
- Consider link rot (prefer stable URLs)
- Add context for why the link is relevant

### Lists

**Ordered lists** for sequential steps:
1. First do this
2. Then do this
3. Finally do this

**Unordered lists** for non-sequential items:
- Feature A
- Feature B
- Feature C

### Tables

Use tables for structured data comparison:

| Feature | Free | Pro |
|---------|------|-----|
| Users   | 5    | 100 |
| Storage | 1GB  | 50GB|

Avoid tables for simple lists or prose content.

### Admonitions

Use consistent patterns for callouts:

```markdown
> **Note:** Additional information

> **Warning:** Important caution

> **Tip:** Helpful suggestion
```

## Review Checklist

When reviewing documentation, evaluate:

### Content Quality
- [ ] Purpose is clear from introduction
- [ ] All claims are accurate and verifiable
- [ ] Examples are complete and working
- [ ] Prerequisites are documented
- [ ] Edge cases are addressed

### Structure
- [ ] Logical flow from introduction to details
- [ ] Appropriate heading hierarchy
- [ ] Related content is grouped together
- [ ] Navigation is intuitive

### Formatting
- [ ] Code blocks have language specified
- [ ] Links are valid and descriptive
- [ ] Lists are used appropriately
- [ ] Tables are well-formatted
- [ ] Consistent style throughout

### Accessibility
- [ ] Images have alt text
- [ ] Color is not sole indicator
- [ ] Content works without images
- [ ] Headings describe content

## Common Issues

### Outdated Content
- **Signs:** Version numbers don't match, deprecated APIs referenced, screenshots show old UI
- **Fix:** Update or remove outdated sections, add "last updated" dates

### Missing Context
- **Signs:** Assumes knowledge not provided, jumps into details, missing prerequisites
- **Fix:** Add introduction, document assumptions, link to prerequisites

### Inconsistent Terminology
- **Signs:** Same concept called different names, abbreviations undefined
- **Fix:** Create glossary, standardize terms, define on first use

### Broken Examples
- **Signs:** Code doesn't compile, commands fail, outputs don't match
- **Fix:** Test all examples, update for current versions, add expected outputs

### Poor Organization
- **Signs:** Related content scattered, unclear navigation, buried important info
- **Fix:** Restructure by topic, add table of contents, move critical info up

## Writing Style

### Voice and Tone
- Use active voice: "Configure the server" not "The server should be configured"
- Be direct: "Run the command" not "You might want to run the command"
- Stay objective: Focus on facts and procedures

### Technical Accuracy
- Verify all technical claims
- Include version numbers where relevant
- Test commands and code before documenting
- Reference authoritative sources

### Audience Awareness
- Define the target audience clearly
- Adjust complexity to match audience
- Provide links for background knowledge
- Don't over-explain basics to experts

## Integration with Site Generators

### MkDocs
- Follow `mkdocs.yml` navigation structure
- Use MkDocs-specific admonition syntax
- Leverage plugins (search, versioning)

### Sphinx
- Use reStructuredText conventions
- Follow cross-reference patterns
- Integrate with autodoc for API docs

### Docusaurus
- Use MDX features appropriately
- Follow sidebar configuration
- Leverage versioning features

## Additional Resources

### Reference Files

For detailed guidance, consult:
- **`references/style-guide.md`** - Comprehensive writing style rules
- **`references/review-criteria.md`** - Detailed review criteria and scoring

### Example Files

Working examples in `examples/`:
- **`good-readme.md`** - Well-structured README template
- **`api-doc-template.md`** - API documentation template
