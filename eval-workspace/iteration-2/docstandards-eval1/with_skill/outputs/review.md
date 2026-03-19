# README Review: Go CLI Tool

## Input Reviewed

A minimal README containing only the project name (e.g., `# mycli`). No other content present.

---

## Summary

The README is essentially a placeholder. It provides no usable information to someone discovering the project. Applying the documentation-standards skill's README checklist, **7 of 8 standard sections are missing entirely**, and the one section that exists (title) lacks a brief description.

---

## Findings by Section

### 1. Title and Brief Description

**Status:** Partially present
**Issue:** The project name is present but there is no description explaining what the tool does, who it is for, or what problem it solves.

**Recommendation:** Add a one-to-two sentence description immediately below the title. State what the CLI does, in active voice, addressed to the reader.

```markdown
# mycli

A command-line tool for [doing X]. Built in Go.
```

---

### 2. Features / Highlights

**Status:** Missing
**Issue:** There is no indication of what the tool offers. A reader cannot evaluate whether this project is relevant to them.

**Recommendation:** Add a short bulleted list of core capabilities. Keep it scannable -- three to six items is typical for a CLI tool.

---

### 3. Installation / Setup

**Status:** Missing
**Issue:** This is the most critical gap for a Go CLI. Without installation instructions, the README fails its primary job. Users need to know:

- Go version requirements (minimum Go version, any build constraints)
- How to install via `go install`
- How to build from source
- Whether pre-built binaries are available

**Recommendation:** Add an Installation section with at least:

```markdown
## Installation

Requires Go 1.21 or later.

go install github.com/yourname/mycli@latest
```

If you distribute binaries, add download links or reference a Releases page.

---

### 4. Quick Start / Usage

**Status:** Missing
**Issue:** There is no example of how to invoke the tool. A user who installs it has no idea what to type next.

**Recommendation:** Show the most common invocation with expected output:

```markdown
## Usage

mycli [command] [flags]

### Example

$ mycli greet --name world
Hello, world!
```

Include the `--help` output or a summary of available subcommands if the CLI has them.

---

### 5. Configuration Options

**Status:** Missing
**Issue:** If the tool accepts flags, environment variables, or config files, none of that is documented.

**Recommendation:** Add a Configuration section covering:

- CLI flags and their defaults
- Environment variables (if any)
- Config file location and format (if any)

A table works well here:

```markdown
| Flag       | Env Var       | Default | Description          |
|------------|---------------|---------|----------------------|
| `--output` | `MYCLI_OUT`   | stdout  | Output destination   |
| `--verbose` | `MYCLI_VERBOSE` | false | Enable verbose logging |
```

---

### 6. API Reference (or Link)

**Status:** Missing
**Issue:** For a Go CLI, this typically means either a link to `pkg.go.dev` documentation or a command reference listing all subcommands.

**Recommendation:** At minimum, link to the generated godoc:

```markdown
## Documentation

See [pkg.go.dev](https://pkg.go.dev/github.com/yourname/mycli) for API documentation.
```

If the CLI has subcommands, consider a command reference table or point users to `mycli help`.

---

### 7. Contributing Guidelines

**Status:** Missing
**Issue:** No information on how to contribute, run tests, or set up a development environment.

**Recommendation:** Add a Contributing section or link to a CONTRIBUTING.md file. At minimum, cover:

- How to clone and build locally (`go build ./...`)
- How to run tests (`go test ./...`)
- Any linting or formatting requirements (`golangci-lint`, `gofmt`)

---

### 8. License

**Status:** Missing
**Issue:** No license information. This is a legal gap -- without a license, the code is technically "all rights reserved" by default.

**Recommendation:** Add a License section and include a LICENSE file in the repository root:

```markdown
## License

MIT License. See [LICENSE](LICENSE) for details.
```

---

## Additional Findings

### Missing Prerequisites

Per the documentation-standards skill, prerequisites should be stated upfront. For a Go CLI, this means:

- Required Go version
- Required OS/architecture (if platform-specific)
- Any external dependencies (e.g., a database, system libraries)

### No Code Examples

The skill emphasizes that working examples build trust. The README should include at least one complete, copy-paste-ready example showing the tool in action from install to first useful output.

### No Badges or Project Metadata

Consider adding:

- Go version badge
- Build status (CI badge)
- Go Report Card
- License badge

These give an immediate signal of project health.

### godoc Compliance

Since this is a Go project, verify that all exported types, functions, and packages have doc comments. The `go doc` and `pkg.go.dev` toolchain will generate API reference automatically from these comments, but only if they exist. Run:

```bash
# Check for missing doc comments
go vet ./...
# Or use a stricter linter
golangci-lint run --enable revive
```

---

## Review Checklist (from documentation-standards skill)

### Content Quality

- [ ] Purpose is clear from introduction -- **FAIL**: no description present
- [ ] All claims are accurate and verifiable -- **N/A**: no claims made
- [ ] Examples are complete and working -- **FAIL**: no examples
- [ ] Prerequisites are documented -- **FAIL**: no prerequisites listed
- [ ] Edge cases are addressed -- **FAIL**: no content to evaluate

### Structure

- [ ] Logical flow from introduction to details -- **FAIL**: no flow, only title
- [ ] Appropriate heading hierarchy -- **PASS**: single H1 is correct
- [ ] Related content is grouped together -- **N/A**: no content
- [ ] Navigation is intuitive -- **FAIL**: nothing to navigate

### Formatting

- [ ] Code blocks have language specified -- **N/A**: no code blocks
- [ ] Links are valid and descriptive -- **N/A**: no links
- [ ] Lists are used appropriately -- **N/A**: no lists
- [ ] Tables are well-formatted -- **N/A**: no tables
- [ ] Consistent style throughout -- **N/A**: no content

### Accessibility

- [ ] Images have alt text -- **N/A**: no images
- [ ] Color is not sole indicator -- **N/A**
- [ ] Content works without images -- **N/A**
- [ ] Headings describe content -- **PARTIAL**: title exists but is not descriptive

---

## Priority Recommendations

Ranked by impact to a new user discovering this project:

1. **Add a description** -- one sentence explaining what the tool does (5 minutes)
2. **Add installation instructions** -- `go install` command plus Go version requirement (10 minutes)
3. **Add a quick-start example** -- show the tool doing something useful (15 minutes)
4. **Add a license** -- pick one and add the file (5 minutes)
5. **Add configuration/flags documentation** -- table of flags and environment variables (20 minutes)
6. **Add contributing guidelines** -- how to build, test, and submit changes (15 minutes)
7. **Add godoc links** -- point to pkg.go.dev for API reference (5 minutes)
8. **Add features list** -- bullet points of what the tool offers (10 minutes)

Total estimated effort: approximately 1.5 hours for a complete, standards-compliant README.
