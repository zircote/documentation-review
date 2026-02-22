---
title: "Getting Started with Documentation Review"
diataxis_type: tutorial
---

# Getting Started with Documentation Review

In this tutorial, we will install the documentation-review plugin for Claude Code, configure it for a project, run a documentation review, and generate a README. By the end, you will have a working documentation workflow and a generated README for your project.

## What you will need

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) installed and working
- A project with at least one existing documentation file (any Markdown file will do)

## Step 1: Install the plugin

We will start by installing the documentation-review plugin from GitHub. In your terminal, run:

```bash
claude plugin install zircote/documentation-review
```

You should see output confirming the plugin was installed successfully. The plugin adds several new slash commands to Claude Code, all prefixed with `/doc-`.

## Step 2: Configure the plugin for your project

Next, we will set up the plugin so it knows about your project's documentation structure. Open Claude Code in your project directory and run:

```
/doc-setup
```

The setup command will walk you through an interactive configuration. It will:

1. Detect your project structure (languages, frameworks, existing docs)
2. Ask you about your documentation paths and preferences
3. Create a configuration file at `.claude/documentation-review.local.md`

Follow the prompts and accept the defaults for now. You will notice that a new file appears in your `.claude/` directory when setup completes.

> **Tip:** You can re-run `/doc-setup` at any time to update your configuration. For more details on customizing the configuration, see the [configuration guide](../guides/configure-project.md).

## Step 3: Review your existing documentation

Now we will run a documentation review to see how the plugin analyzes your project's docs. Run:

```
/doc-review
```

The plugin will scan all documentation files in your configured paths and produce a review report. The output should look something like this:

```
## Documentation Review Report

### Files Reviewed
- README.md
- docs/setup.md

### Issues Found
- README.md: Missing installation instructions (severity: high)
- docs/setup.md: Outdated version references (severity: medium)

### Summary
Reviewed 2 files. Found 2 issues (1 high, 1 medium).
```

Your report will differ depending on your project, but you will see a structured breakdown of issues grouped by severity. Each issue includes the file, a description, and a severity level.

You can also review a specific file by passing a path:

```
/doc-review README.md
```

This narrows the review to just that file, which is useful when you want to check a single document after making changes.

## Step 4: Generate a README

Now we will use the plugin to generate a new README based on your codebase. Run:

```
/doc-create readme
```

The plugin will analyze your project's code, configuration files, and existing documentation, then generate a README.md file. You will see it examine your project structure and produce a comprehensive README with sections like:

- Project title and description
- Installation instructions
- Usage examples
- Configuration options

Review the generated README and make any adjustments you like. The plugin creates a solid starting point that you can refine.

## What you have accomplished

In this tutorial, you have:

- Installed the documentation-review plugin for Claude Code
- Configured the plugin for your project with `/doc-setup`
- Run a documentation review and read the report with `/doc-review`
- Generated a README from your codebase with `/doc-create readme`

## Next steps

Now that you have the basics working, explore these guides for deeper workflows:

- [Review documentation](../guides/review-documentation.md) -- learn how to run targeted reviews, filter by severity, and act on findings
- [Create documentation](../guides/create-documentation.md) -- generate API docs, templates, and other document types
- [Configure your project](../guides/configure-project.md) -- customize paths, standards, and review behavior
- [Understanding documentation quality](../explanation/documentation-quality.md) -- learn what makes documentation effective and how the plugin evaluates it
