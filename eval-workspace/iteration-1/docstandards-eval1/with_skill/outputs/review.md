# Documentation Review: CLI Tool README

**File reviewed:** `./README.md`
**Current state:** Contains only the project name (e.g., `# mycli`)
**Review date:** 2026-03-19

---

## Overall Assessment

**Rating: Critically Incomplete**

The README contains a single H1 heading with the project name and nothing else. For a Go CLI tool, this leaves users with no way to understand what the tool does, how to install it, or how to use it. Every standard README section is missing.

---

## Review Findings

### 1. Missing: Brief Description

**Severity:** High
**Standard:** README files must open with a title and brief description (Documentation Standards, Document Structure, item 1).

The project name alone tells readers nothing. Add one to two sentences directly below the H1 that explain what the tool does and who it is for.

**Recommended fix:**

```markdown
# mycli

A command-line tool for [what it does]. Built in Go.
```

---

### 2. Missing: Features / Highlights

**Severity:** High
**Standard:** README files should include a Features/highlights section (Document Structure, item 2).

Users scanning the README need a quick list of what the tool offers. Without this, they cannot evaluate whether the tool fits their needs.

**Recommended fix:** Add an H2 `## Features` section with a bulleted list of key capabilities.

---

### 3. Missing: Installation / Setup

**Severity:** Critical
**Standard:** README files must include Installation/setup instructions (Document Structure, item 3).

For a Go CLI tool, this section should cover at minimum:
- `go install` command with the full module path and version
- Binary download instructions (if releases are published)
- Build-from-source steps (`git clone`, `go build`)
- Any system requirements or prerequisites (minimum Go version, OS support)

**Recommended fix:**

```markdown
## Installation

### From source

Requires Go 1.21 or later.

```bash
go install github.com/yourorg/mycli@latest
```

### Binary releases

Download pre-built binaries from the [Releases](https://github.com/yourorg/mycli/releases) page.
```

---

### 4. Missing: Quick Start / Usage

**Severity:** Critical
**Standard:** README files must include Quick start/usage (Document Structure, item 4).

Users need to see how to run the tool immediately after installing it. Show the most common invocation with expected output.

**Recommended fix:**

```markdown
## Quick Start

```bash
mycli --help
mycli run --input data.json
```
```

Include a brief explanation of the primary workflow and at least one concrete example with expected output.

---

### 5. Missing: Configuration Options

**Severity:** Medium
**Standard:** README files should cover Configuration options (Document Structure, item 5).

If the tool accepts flags, environment variables, or a config file, document them. For a CLI tool, a flags table or a link to `--help` output is appropriate.

**Recommended fix:**

```markdown
## Configuration

| Flag | Environment Variable | Default | Description |
|------|---------------------|---------|-------------|
| `--config` | `MYCLI_CONFIG` | `~/.mycli.yaml` | Path to config file |
| `--verbose` | `MYCLI_VERBOSE` | `false` | Enable verbose output |
```

---

### 6. Missing: API Reference or Command Reference

**Severity:** Medium
**Standard:** README files should include API reference or a link to it (Document Structure, item 6).

For a CLI tool, this means a command/subcommand reference. Either document commands inline or link to generated docs (e.g., from `godoc` or a docs site).

**Recommended fix:** Add a `## Commands` section listing all subcommands with brief descriptions, or link to a dedicated reference page.

---

### 7. Missing: Contributing Guidelines

**Severity:** Low
**Standard:** README files should include Contributing guidelines (Document Structure, item 7).

Even a minimal section helps potential contributors know how to get involved.

**Recommended fix:**

```markdown
## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for development setup and guidelines.
```

---

### 8. Missing: License

**Severity:** Medium
**Standard:** README files must include License information (Document Structure, item 8).

Users and organizations need to know the license before they can use the tool. If a LICENSE file exists, reference it.

**Recommended fix:**

```markdown
## License

[MIT](LICENSE)
```

---

### 9. Missing: Go-Specific Documentation

**Severity:** Medium
**Standard:** All public API must be documented; doc toolchain build must pass clean (Language Doc Toolchains, principles 1-3).

For a Go project:
- Ensure all exported functions, types, and packages have `godoc` comments.
- Run `go doc ./...` to verify coverage.
- Consider adding a Go Reference badge: `[![Go Reference](https://pkg.go.dev/badge/github.com/yourorg/mycli.svg)](https://pkg.go.dev/github.com/yourorg/mycli)`

---

### 10. Missing: Prerequisites

**Severity:** Medium
**Standard:** Include prerequisites and requirements upfront (Core Quality Principles, Completeness).

State the minimum Go version, supported operating systems, and any external dependencies (databases, services, system libraries).

---

## Checklist Summary

### Content Quality
- [ ] Purpose is clear from introduction -- **FAIL**: No description present
- [ ] All claims are accurate and verifiable -- **N/A**: No claims made
- [ ] Examples are complete and working -- **FAIL**: No examples present
- [ ] Prerequisites are documented -- **FAIL**: No prerequisites listed
- [ ] Edge cases are addressed -- **FAIL**: No content at all

### Structure
- [ ] Logical flow from introduction to details -- **FAIL**: No flow, single heading only
- [ ] Appropriate heading hierarchy -- **PARTIAL**: H1 exists, no other headings
- [ ] Related content is grouped together -- **FAIL**: No content to group
- [ ] Navigation is intuitive -- **FAIL**: Nothing to navigate

### Formatting
- [ ] Code blocks have language specified -- **N/A**: No code blocks
- [ ] Links are valid and descriptive -- **N/A**: No links
- [ ] Lists are used appropriately -- **N/A**: No lists
- [ ] Tables are well-formatted -- **N/A**: No tables
- [ ] Consistent style throughout -- **N/A**: No content

### Accessibility
- [ ] Images have alt text -- **N/A**: No images
- [ ] Color is not sole indicator -- **N/A**
- [ ] Content works without images -- **N/A**
- [ ] Headings describe content -- **PARTIAL**: Title exists but is the only heading

---

## Recommended README Skeleton

Based on the standards, here is the structure to build out:

```markdown
# mycli

Brief description of what the tool does and its primary value.

## Features

- Feature one
- Feature two
- Feature three

## Installation

### From source

Requires Go X.Y or later.

\`\`\`bash
go install github.com/yourorg/mycli@latest
\`\`\`

### Binary releases

Download from [Releases](https://github.com/yourorg/mycli/releases).

## Quick Start

\`\`\`bash
mycli [command] [flags]
\`\`\`

## Commands

| Command | Description |
|---------|-------------|
| `run`   | Does the main thing |
| `init`  | Sets up configuration |

## Configuration

Describe flags, environment variables, and config file format.

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md).

## License

[MIT](LICENSE)
```

---

## Priority Actions

1. **Immediate**: Add a one-line description below the project title.
2. **Immediate**: Add Installation and Quick Start sections with working commands.
3. **Soon**: Add Features, Commands, and Configuration sections.
4. **Soon**: Add License section and verify a LICENSE file exists.
5. **Later**: Add Contributing guidelines and Go Reference badge.
6. **Later**: Audit `godoc` coverage on all exported symbols.
