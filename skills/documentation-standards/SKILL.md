---
name: documentation-standards
description: This skill should be used when the user asks to "review documentation", "improve docs quality", "check markdown formatting", "fix documentation structure", "write better docs", "documentation best practices", or needs guidance on documentation quality, structure, formatting, or technical writing standards.
version: 0.1.0
---

# Documentation Standards

<!-- BEGIN MNEMONIC PROTOCOL -->
## Memory Operations

You have PERSISTENT MEMORY across sessions.

BEFORE starting any task:
```bash
if [ -d ~/.claude/mnemonic ]; then
    rg -i "{documentation_standards}" ~/.claude/mnemonic/ --glob "*.memory.md" -l | head -5
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

## Language Doc Toolchains

Language-native documentation toolchains generate API reference from source code comments. When a project uses one, in-source doc comments are the authoritative API documentation — not separate markdown files.

### Rust — rustdoc

**Build command:** `cargo doc --no-deps --all-features`

**Doc comment conventions:**
- Use `///` for item docs, `//!` for module/crate docs
- Required sections for public items: `# Arguments`, `# Returns`, `# Errors`, `# Examples`
- Code in `# Examples` is compiled and tested by `cargo test` (doctests)
- Use `#[doc(hidden)]` to exclude internal items
- Intra-doc links: `` [`OtherType`] `` or `` [`crate::module::Type`] ``

**Review criteria:**
- All public items (`pub fn`, `pub struct`, `pub enum`, `pub trait`) must have doc comments
- `cargo doc --no-deps --all-features` must build without warnings
- Doctests must pass (`cargo test --doc`)
- Module-level docs (`//!`) should provide overview and usage context
- `# Safety` section required for `unsafe` functions
- `# Panics` section required when a function can panic

**Configuration in `documentation-review.local.md`:**
```yaml
api_docs:
  rustdoc:
    enabled: true
    build_command: "cargo doc --no-deps --all-features"
    crates:
      - my-crate
    warn_missing_docs: true
```

### Go — godoc / pkgsite

**Build command:** `go doc ./...` (local) or hosted on pkg.go.dev

**Doc comment conventions:**
- Package comment: block comment before `package` declaration
- Function/type comment: line comment directly above declaration, starting with the name
- Use complete sentences, start with the function/type name
- Code examples in `_test.go` files with `Example` prefix functions

**Review criteria:**
- Every exported function, type, and package must have a doc comment
- Comments must start with the declared name: `// FuncName does...`
- `go vet` should pass without doc-related warnings
- Example functions (`ExampleFuncName`) should exist for key APIs
- Package doc should include usage overview

**Configuration:**
```yaml
api_docs:
  godoc:
    enabled: true
    packages:
      - ./...
```

### Python — Sphinx autodoc / pydoc

**Build command:** `sphinx-build -b html docs/ docs/_build/` or `pdoc --html .`

**Doc comment conventions:**
- Use docstrings (triple-quoted strings) on modules, classes, functions
- Formats: Google style, NumPy style, or reStructuredText (Sphinx native)
- Type hints complement docstrings but don't replace descriptions

**Google style example:**
```python
def fetch(url: str, timeout: int = 30) -> Response:
    """Fetch a resource from the given URL.

    Args:
        url: The URL to fetch.
        timeout: Request timeout in seconds.

    Returns:
        The HTTP response object.

    Raises:
        ConnectionError: If the server is unreachable.
    """
```

**Review criteria:**
- All public modules, classes, and functions must have docstrings
- Args, Returns, Raises sections populated for public functions
- `sphinx-build` (if used) must build without warnings
- Consistent docstring format across the project (Google, NumPy, or rST — not mixed)

**Configuration:**
```yaml
api_docs:
  sphinx:
    enabled: true
    build_command: "sphinx-build -b html docs/ docs/_build/"
    docstring_style: google  # google | numpy | rst
```

### TypeScript/JavaScript — TypeDoc / JSDoc / TSDoc

**Build command:** `npx typedoc --out docs/api src/index.ts`

**Doc comment conventions:**
- Use `/** ... */` JSDoc/TSDoc block comments
- `@param`, `@returns`, `@throws`, `@example`, `@deprecated`
- TypeScript types reduce need for `@type` annotations but descriptions remain important
- TSDoc is the TypeScript-specific standard (stricter than JSDoc)

**Review criteria:**
- All exported functions, classes, and interfaces must have doc comments
- `@param` for each parameter with description
- `@returns` with description
- `@example` for key APIs
- `typedoc` (if used) must build without errors

**Configuration:**
```yaml
api_docs:
  typedoc:
    enabled: true
    build_command: "npx typedoc"
    entry_points:
      - src/index.ts
```

### Java/Kotlin — Javadoc / Dokka / KDoc

**Build command:** `javadoc -d docs/api src/**/*.java` or `./gradlew dokkaHtml`

**Doc comment conventions:**
- Javadoc: `/** ... */` with `@param`, `@return`, `@throws`, `@see`, `@since`, `@deprecated`
- KDoc (Kotlin): `/** ... */` with `@param`, `@return`, `@throws`, `@property`, `@constructor`
- First sentence is the summary — keep it concise

**Review criteria:**
- All public classes, methods, and fields must have doc comments
- `@param` for each parameter, `@return` for non-void methods
- `@throws`/`@exception` for checked exceptions
- Javadoc/Dokka build must complete without errors or warnings
- `@since` tag on new public API additions

**Configuration:**
```yaml
api_docs:
  javadoc:
    enabled: true
    build_command: "./gradlew javadoc"
  # Or for Kotlin:
  dokka:
    enabled: true
    build_command: "./gradlew dokkaHtml"
```

### Swift — DocC

**Build command:** `swift package generate-documentation`

**Doc comment conventions:**
- Use `///` line comments or `/** ... */` block comments
- Markdown-based with `- Parameters:`, `- Returns:`, `- Throws:`
- DocC supports tutorials, articles, and symbol documentation in a unified system

**Review criteria:**
- All public symbols must have doc comments
- Parameters, return values, and thrown errors documented
- DocC build must complete without warnings

### C# / .NET — XML Doc Comments / DocFX

**Build command:** `docfx build` or compiler-generated XML

**Doc comment conventions:**
- Use `///` with XML tags: `<summary>`, `<param>`, `<returns>`, `<exception>`, `<example>`
- Enable `<GenerateDocumentationFile>true</GenerateDocumentationFile>` in `.csproj`

**Review criteria:**
- All public types and members must have `<summary>` documentation
- `<param>` for each parameter, `<returns>` for non-void methods
- Build with `/warnaserror:CS1591` to enforce doc comments

### Elixir — ExDoc

**Build command:** `mix docs`

**Doc comment conventions:**
- Use `@moduledoc` for module docs, `@doc` for function docs
- Markdown supported in doc attributes
- `@typedoc` for type documentation
- Doctests with `iex>` prefix are compiled and tested

**Review criteria:**
- All public modules and functions must have `@moduledoc`/`@doc`
- `mix docs` must build without warnings
- Doctests should pass

### General Principles for All Toolchains

1. **Source comments are the API reference** — when a language doc toolchain is active, in-source doc comments are the authoritative documentation, not separate markdown files
2. **Build must pass clean** — the doc toolchain build command must complete without warnings or errors
3. **All public API must be documented** — exported/public symbols without doc comments are a review finding
4. **Examples should be testable** — prefer doctests or example functions that are compiled/executed during testing
5. **Consistent format** — pick one doc comment style per project and enforce it

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

### Astro (Starlight)
- Use Starlight for documentation sites (`@astrojs/starlight`)
- Content collections in `src/content/docs/` with frontmatter (`title`, `description`, `sidebar`)
- MDX and Markdoc support for interactive components
- Built-in i18n, search, and sidebar generation from file structure
- Leverage component islands for interactive API playgrounds
- Use `astro.config.mjs` Starlight config for navigation, social links, and customization
- Frontmatter schema validation via Zod in `src/content/config.ts`

## Additional Resources

### Reference Files

For detailed guidance, consult:
- **`references/style-guide.md`** - Comprehensive writing style rules
- **`references/review-criteria.md`** - Detailed review criteria and scoring

### Example Files

Working examples in `examples/`:
- **`good-readme.md`** - Well-structured README template
- **`api-doc-template.md`** - API documentation template
