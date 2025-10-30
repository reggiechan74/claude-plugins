---
name: hooks
description: How to create and configure hooks in Claude Code for automated validation, transformations, and lifecycle event handling. Use when user asks about hooks, event automation, pre/post tool execution, session management, or automated workflows.
---

# Claude Code Hooks

## Overview

Claude Code hooks are automated scripts that execute at specific lifecycle events, enabling validation, transformation, and control over tool execution and session management.

## Configuration Structure

Hooks are configured in settings files (`~/.claude/settings.json`, `.claude/settings.json`, or `.claude/settings.local.json`) using this pattern:

```json
{
  "hooks": {
    "EventName": [
      {
        "matcher": "ToolPattern",
        "hooks": [
          {
            "type": "command",
            "command": "your-command-here",
            "timeout": 60
          }
        ]
      }
    ]
  }
}
```

**Key features:**
- Matchers use case-sensitive patterns (regex supported)
- Use `*` or empty string to match all tools
- Optional timeout configuration (default: 60 seconds)
- `$CLAUDE_PROJECT_DIR` environment variable available for project-relative paths

## Hook Events

### PreToolUse & PostToolUse
Executes before/after tool operations. Supports matchers for:
- Task, Bash, Glob, Grep, Read, Edit, Write, WebFetch, WebSearch

### UserPromptSubmit
Runs when the user submits a prompt, before Claude processes it, enabling context injection and prompt validation.

### Notification
Triggers when Claude requests permissions or waits for input.

### Stop & SubagentStop
Executes when agents complete responses.

### SessionStart
Useful for loading in development context like existing issues or recent changes to your codebase, installing dependencies, or setting up environment variables.

**Environment persistence:**
Use `CLAUDE_ENV_FILE` to persist variables across bash commands:
```bash
echo 'export NODE_ENV=production' >> "$CLAUDE_ENV_FILE"
```

### SessionEnd
Runs during session cleanup with `reason` field (clear, logout, prompt_input_exit, other).

### PreCompact
Executes before context compaction with matchers: `manual` or `auto`.

## Hook Input/Output

**Input delivered via stdin as JSON containing:**
- session_id, transcript_path, cwd, permission_mode
- hook_event_name and event-specific fields

**Output methods:**

1. **Exit codes:**
   - 0: Success (stdout shown in transcript mode)
   - 2: Blocking error (stderr fed back to Claude)
   - Other: Non-blocking error

2. **JSON output** for advanced control:
```json
{
  "continue": true,
  "stopReason": "message",
  "suppressOutput": true,
  "systemMessage": "warning"
}
```

## Decision Control Examples

**PreToolUse:** Allow, deny, or ask for tool execution with optional input modification:
```json
{
  "hookSpecificOutput": {
    "permissionDecision": "allow",
    "permissionDecisionReason": "Auto-approved",
    "updatedInput": {"field": "value"}
  }
}
```

**PostToolUse:** Provide feedback or block:
```json
{
  "decision": "block",
  "reason": "Explanation"
}
```

**UserPromptSubmit:** Block prompts or add context:
```json
{
  "decision": "block",
  "reason": "Security violation"
}
```

**Stop/SubagentStop:** Prevent completion:
```json
{
  "decision": "block",
  "reason": "Must continue with..."
}
```

## MCP Tool Integration

MCP tools follow pattern: `mcp__<server>__<tool>`

Configure with regex matchers:
```json
{
  "matcher": "mcp__memory__.*",
  "hooks": [{"type": "command", "command": "validate.py"}]
}
```

## Practical Examples

**Bash validation (exit code):**
Detect non-preferred commands and reject them with exit code 2.

**UserPromptSubmit context injection (exit code 0):**
Add current time or project context via stdout; Claude sees this automatically.

**PreToolUse approval (JSON):**
Auto-approve documentation file reads while maintaining security audit trails.

## Common Use Cases

- **Notifications**: Customize input/permission alerts
- **Automatic formatting**: Run `prettier` on TypeScript, `gofmt` on Go files after edits
- **Logging**: Track executed commands for compliance
- **Feedback**: Automated codebase convention validation
- **Custom permissions**: Block production/sensitive file modifications

## Execution Details

- **Timeout:** 60 seconds default, configurable per command
- **Parallelization:** All matching hooks run simultaneously
- **Deduplication:** Identical commands execute once
- **Environment:** Runs in current directory with Claude's environment

## Security Considerations

**Critical warning:** Claude Code hooks execute arbitrary shell commands on your system automatically. By using hooks, you acknowledge that:
- You are solely responsible for the commands you configure
- Hooks can modify, delete, or access any files your user account can access
- Malicious or poorly written hooks can cause data loss or system damage

**Best practices:**
- Validate and sanitize inputs
- Always quote shell variables (`"$VAR"`)
- Block path traversal attempts
- Use absolute paths
- Avoid sensitive files (.env, .git, credentials)

Configuration snapshots prevent mid-session modifications from affecting behavior.

## Debugging

Use `claude --debug` for hook execution details showing matched patterns, command execution, status codes, and output. Progress messages display in transcript mode (Ctrl-R).

## Configuration via Slash Command

Use the `/hooks` slash command to configure hooks interactively and save to either user settings (all projects) or project-specific settings.
