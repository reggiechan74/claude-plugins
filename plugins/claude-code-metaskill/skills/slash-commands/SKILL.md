---
name: slash-commands
description: How to create, use, and manage custom slash commands in Claude Code. Use when user asks about creating custom commands, command arguments, command organization, or slash command features.
---

# Slash Commands

## Overview

Slash commands control Claude's behavior during interactive sessions. They're organized into built-in commands, custom commands defined by users, plugin commands, and MCP-exposed prompts.

## Built-in Commands

Key built-in commands include:

- `/help` - Get usage help
- `/clear` - Clear conversation history
- `/model` - Select or change the AI model
- `/cost` - Show token usage statistics
- `/memory` - Edit CLAUDE.md memory files
- `/sandbox` - Enable sandboxed bash execution
- `/rewind` - Rewind conversation and/or code
- `/mcp` - Manage MCP server connections

## Custom Slash Commands

### File Organization

Custom commands are Markdown files stored in two locations:

- **Project commands**: `.claude/commands/` (shared with team via git)
- **Personal commands**: `~/.claude/commands/` (user-level only)

### Basic Syntax

```
/<command-name> [arguments]
```

### Arguments

Commands support two argument styles:

**Capture all arguments:**
```markdown
Fix issue #$ARGUMENTS following coding standards
```

**Individual positional arguments:**
```markdown
Review PR #$1 with priority $2 and assign to $3
```

### Features

**Namespacing**: Organize commands in subdirectories. A file at `.claude/commands/frontend/component.md` creates `/component` with description "(project:frontend)".

**Bash execution**: Prefix commands with `!` to execute bash and include output:
```markdown
---
allowed-tools: Bash(git status:*), Bash(git add:*)
---
Current git status: ![git status]
```

Note: Replace `[git status]` with the actual command in backticks when creating your slash command.

**File references**: Use `@` prefix to include file contents in commands:
```markdown
Review the implementation in @src/utils/helpers.js
```

**Frontmatter metadata**:
```markdown
---
description: Brief command description
allowed-tools: Tool specifications
argument-hint: [expected-args]
model: specific-model-id
---
```

## Plugin Commands

Plugins distribute custom slash commands automatically. They use the format `/plugin-name:command-name` when disambiguation is needed, support all standard features (arguments, bash execution, file references), and appear in `/help` when installed.

## MCP Slash Commands

MCP servers expose prompts as commands with the pattern:

```
/mcp__<server-name>__<prompt-name> [arguments]
```

These are dynamically discovered from connected MCP servers and automatically available when the server is active.

## SlashCommand Tool

The `SlashCommand` tool allows Claude to invoke custom commands programmatically during conversations. To encourage usage, reference commands in your instructions with their slash prefix (e.g., "Run `/write-unit-test`").

**Limitations**: Only supports user-defined custom commands with a `description` field populated in frontmatter. Built-in commands like `/compact` cannot be invoked via the tool.

**Management**:
- Disable all: `/permissions` then add `SlashCommand` to deny rules
- Disable specific commands: Add `disable-model-invocation: true` to frontmatter
- Permission syntax: `SlashCommand:/commit` (exact match) or `SlashCommand:/review-pr:*` (prefix match)

## Skills vs Slash Commands

**Use slash commands for**: Simple, frequently-used prompts that fit in one file.

**Use Skills for**: Complex capabilities requiring multiple files, scripts, or organizational structure.

Key difference: Commands require explicit invocation; Skills are discovered automatically based on context.
