---
name: documentation-standards
description: >-
  Standards and patterns for creating, reviewing, and improving technical documentation —
  covering markdown formatting, heading hierarchy, code blocks, links, writing style,
  accessibility, and language-native doc toolchains (rustdoc, godoc, Sphinx, TypeDoc, Javadoc,
  DocC, ExDoc). Use this skill whenever the user wants to review documentation quality, audit
  markdown formatting, fix document structure, improve technical writing, check doc comment
  coverage, enforce doc standards, write better docs, or asks about documentation best practices.
  Also triggers for language-specific doc toolchain questions (rustdoc, godoc, pydoc, JSDoc/TSDoc,
  Javadoc/KDoc, DocC, ExDoc) and site generator integration (MkDocs, Sphinx, Docusaurus, Astro
  Starlight). If the user mentions "docs", "documentation", "README", "writing style",
  "doc comments", "markdown", or "technical writing" in a quality or standards context, this
  skill likely applies.
---

# Documentation Standards

Standards and patterns for creating, reviewing, and improving technical documentation. Good documentation reduces support burden, accelerates onboarding, and builds user trust — these standards exist to make that happen consistently.

## Core Quality Principles

Documentation fails readers in predictable ways: unclear language forces re-reading, missing context creates dead ends, stale examples break trust, and inconsistent terminology confuses. These four principles address the root causes.

### Clarity
- Write for the reader's level of expertise
- Define technical terms on first use
- Use active voice and direct language — "Configure the server" not "The server should be configured"
- Avoid jargon unless the audience expects it

### Completeness
- Cover all essential topics for the document's purpose
- Include prerequisites and requirements upfront
- Provide working examples for technical concepts
- Document edge cases, limitations, and known issues

### Accuracy
- Verify all technical details against current implementation
- Test code examples before including them — broken examples erode trust faster than missing docs
- Update documentation when code changes
- Remove or clearly mark deprecated content

### Consistency
- Follow established style guides (project or industry standard)
- Use consistent terminology throughout — same concept, same name everywhere
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

Headings serve as both navigation and outline — screen readers, search engines, and table-of-contents generators all depend on correct hierarchy.

- H1: Document title (one per document)
- H2: Major sections
- H3: Subsections
- H4: Minor subsections (use sparingly)
- Avoid H5/H6 — if you need them, the document likely needs restructuring into separate pages

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

Effective technical writing is invisible — readers absorb the information without noticing the prose. These patterns make that happen.

### Voice and Tone
- Use active voice: "Configure the server" not "The server should be configured"
- Be direct: "Run the command" not "You might want to run the command"
- Stay objective: Focus on facts and procedures
- Use imperative form for instructions: "Install the package" not "You should install the package"

### Technical Accuracy
- Verify all technical claims against current implementation
- Include version numbers where relevant — readers need to know if docs match their setup
- Test commands and code before documenting
- Reference authoritative sources

### Audience Awareness
- Define the target audience clearly — who is this document for?
- Adjust complexity to match audience expertise
- Provide links for background knowledge rather than inline explanations
- Don't over-explain basics to experts — it signals the docs aren't for them

## Language Doc Toolchains

Language-native documentation toolchains (rustdoc, godoc, Sphinx, TypeDoc, Javadoc/Dokka, DocC, DocFX, ExDoc) generate API reference from source code comments. When a project uses one, in-source doc comments are the authoritative API documentation — not separate markdown files.

**General principles:**
1. Source comments are the API reference — not separate markdown files
2. Doc toolchain build must pass clean without warnings or errors
3. All public API must be documented — missing doc comments are a review finding
4. Examples should be testable (doctests, example functions)
5. Pick one doc comment style per project and enforce it

For per-language conventions, build commands, review criteria, and configuration examples, see **`references/language-toolchains.md`**.

## Integration with Site Generators

When a project uses a documentation site generator, follow its conventions:

| Generator | Key Files | Notes |
|-----------|-----------|-------|
| MkDocs | `mkdocs.yml` | Admonition syntax, search/versioning plugins |
| Sphinx | `conf.py` | reStructuredText, autodoc integration |
| Docusaurus | `docusaurus.config.js` | MDX support, sidebar config, versioning |
| Astro Starlight | `astro.config.mjs` | Content collections in `src/content/docs/`, i18n, component islands |

## Additional Resources

### Reference Files

For detailed guidance, consult:
- **`references/style-guide.md`** - Comprehensive writing style rules
- **`references/review-criteria.md`** - Detailed review criteria and scoring
- **`references/language-toolchains.md`** - Per-language doc toolchain conventions (Rust, Go, Python, TypeScript, Java/Kotlin, Swift, C#, Elixir)

### Example Files

Working examples in `examples/`:
- **`good-readme.md`** - Well-structured README template
- **`api-doc-template.md`** - API documentation template
