# Language Doc Toolchains Reference

Language-native documentation toolchains generate API reference from source code comments. When a project uses one, in-source doc comments are the authoritative API documentation — not separate markdown files.

## Rust — rustdoc

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

## Go — godoc / pkgsite

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

## Python — Sphinx autodoc / pydoc

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

## TypeScript/JavaScript — TypeDoc / JSDoc / TSDoc

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

## Java/Kotlin — Javadoc / Dokka / KDoc

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

## Swift — DocC

**Build command:** `swift package generate-documentation`

**Doc comment conventions:**
- Use `///` line comments or `/** ... */` block comments
- Markdown-based with `- Parameters:`, `- Returns:`, `- Throws:`
- DocC supports tutorials, articles, and symbol documentation in a unified system

**Review criteria:**
- All public symbols must have doc comments
- Parameters, return values, and thrown errors documented
- DocC build must complete without warnings

## C# / .NET — XML Doc Comments / DocFX

**Build command:** `docfx build` or compiler-generated XML

**Doc comment conventions:**
- Use `///` with XML tags: `<summary>`, `<param>`, `<returns>`, `<exception>`, `<example>`
- Enable `<GenerateDocumentationFile>true</GenerateDocumentationFile>` in `.csproj`

**Review criteria:**
- All public types and members must have `<summary>` documentation
- `<param>` for each parameter, `<returns>` for non-void methods
- Build with `/warnaserror:CS1591` to enforce doc comments

## Elixir — ExDoc

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

## General Principles for All Toolchains

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
