# Copilot Instructions

You are working in the Documentation Review plugin repository for Claude Code.

## Project Overview

This is a Claude Code plugin for comprehensive documentation management: review, create, update, and maintain high-quality technical documentation.

## Key Components

- **Skills**: 3 skills covering documentation standards, changelog management, and API documentation
- **Commands**: 6 commands for documentation lifecycle management
- **Agents**: 2 agents for documentation writing and review

## Plugin Structure

```
.claude-plugin/plugin.json  # Plugin manifest
skills/                     # 3 skill directories with SKILL.md and references
commands/                   # 6 documentation commands
agents/                     # 2 documentation agents
templates/                  # Configuration templates
```

## Development Guidelines

1. Follow Claude Code plugin standards
2. Keep changes focused and reviewable
3. Update CHANGELOG.md for user-facing changes
4. Test commands and agent triggering locally

## Testing

```bash
claude --plugin-dir .
```

Then test:
- `/doc-review` command
- `/doc-create` command
- `/doc-update` command
- `/changelog` command
- Agent triggering for documentation tasks
