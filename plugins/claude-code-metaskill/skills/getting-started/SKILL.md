---
name: getting-started
description: Quickstart guide for installing and using Claude Code for the first time. Use when user asks about installation, setup, getting started, or basic usage.
---

# Claude Code Quickstart

## Installation

### NPM Installation

**Requirements:** Node.js 18+

```bash
npm install -g @anthropic-ai/claude-code
```

### Native Installer (Beta)

**Homebrew (macOS):**
```bash
brew install --cask claude-code
```

**macOS/Linux/WSL:**
```bash
curl -fsSL https://claude.ai/install.sh | bash
```

**Windows PowerShell:**
```powershell
irm https://claude.ai/install.ps1 | iex
```

### Verify Installation

```bash
claude --version
```

## Authentication

### First Login

Start an interactive session and authenticate:
```bash
claude
/login
```

### Authentication Options

**Claude.ai (Recommended):**
- Subscription-based
- Easiest setup
- Best for individual developers

**Claude Console:**
- API access with prepaid credits
- Good for teams and enterprises
- Requires API key setup

**Note:** Credentials are stored securely and you won't need to log in again unless you explicitly log out.

## Basic Workflow

### 1. Start a Session

Navigate to your project directory:
```bash
cd /path/to/your/project
claude
```

### 2. Explore Your Codebase

Ask natural language questions:
```
"what does this project do?"
"where is the main entry point?"
"how does authentication work?"
"show me the API endpoints"
```

### 3. Make Code Changes

Request modifications:
```
"add a new function to validate email addresses"
"refactor the user controller for better error handling"
"add unit tests for the authentication module"
```

**Approval process:**
- Claude requests approval before modifying files
- Review changes in the diff viewer
- Approve individually or enable "Accept all" mode

### 4. Git Operations

Use conversational prompts:
```
"what files have I changed?"
"commit my changes with a descriptive message"
"create a pull request for this feature"
```

## Essential Commands

### Interactive Mode

| Command | Purpose |
|---------|---------|
| `claude` | Start interactive mode |
| `claude "task"` | Run single task then exit |
| `/help` | Show available commands |
| `/clear` | Clear conversation history |
| `/memory` | Edit CLAUDE.md memory files |
| `/permissions` | Configure tool permissions |
| `/model` | Select AI model |

### Keyboard Shortcuts

| Shortcut | Action |
|----------|--------|
| `Ctrl+C` | Cancel current operation |
| `Ctrl+D` | Exit Claude Code |
| `Ctrl+L` | Clear screen (keeps history) |
| `Esc Esc` | Rewind code/conversation |
| `↑`/`↓` | Navigate command history |
| `Tab` | Toggle extended thinking |
| `?` | Show help |

## Common First Tasks

### Understand a Project

```
"Explain the project structure"
"What technologies are used here?"
"Where should I start if I want to add a new feature?"
```

### Fix Issues

```
"Find and fix any TypeScript errors"
"Resolve linting issues"
"Fix the failing tests"
```

### Add Features

```
"Add input validation to the signup form"
"Create a new API endpoint for user profile updates"
"Implement pagination for the product list"
```

### Improve Code

```
"Refactor this component for better readability"
"Add error handling to all API calls"
"Improve test coverage"
```

## Pro Tips

### Be Specific

**❌ Vague:**
```
"make this better"
"fix the code"
```

**✅ Specific:**
```
"refactor this function to reduce complexity"
"fix the null pointer exception in getUserProfile"
"add JSDoc comments to all exported functions"
```

### Break Down Complex Tasks

Instead of:
```
"Build a complete user management system"
```

Try:
```
1. "Create a user model with validation"
2. "Add CRUD endpoints for users"
3. "Implement authentication middleware"
4. "Add unit tests for user operations"
```

### Use Context Efficiently

**Attach relevant files:**
```
"Review @src/utils/validation.ts for security issues"
```

**Reference specific code:**
```
"Optimize the database query in getUsersByRole function"
```

### Manage Permissions

Start cautious, then adjust:
1. Begin with manual approval mode
2. Review Claude's changes carefully
3. Enable auto-accept for trusted operations
4. Configure specific permissions via `/permissions`

## Next Steps

### Learn More Features

1. **Custom Commands:** Create slash commands for frequent tasks
2. **Memory Files:** Set up CLAUDE.md for project guidelines
3. **Subagents:** Configure specialized agents for specific tasks
4. **Hooks:** Automate workflows with event-driven scripts
5. **Plugins:** Install marketplace plugins for extended functionality

### Configure Your Workflow

**Create `.claude/CLAUDE.md`:**
```markdown
# Project Guidelines

## Code Style
- Use TypeScript strict mode
- Prefer functional components
- Follow existing naming conventions

## Testing
- Write tests for all new features
- Maintain >80% coverage

## Git
- Use conventional commits
- Squash before merging
```

**Set up permissions in `.claude/settings.json`:**
```json
{
  "permissions": {
    "allow": [
      "Bash(npm run *)",
      "Bash(git status:*)"
    ],
    "deny": [
      "Read(.env*)",
      "Bash(rm:*)"
    ]
  }
}
```

### Explore Advanced Features

**Headless mode for automation:**
```bash
claude -p "run tests and fix failures" \
  --allowedTools "Bash,Read,Edit,Write" \
  --output-format json
```

**Session management:**
```bash
# Continue previous session
claude --continue "add more tests"

# Resume specific session
claude --resume <session-id> "finish the refactoring"
```

**Custom models:**
```bash
claude --model opus  # For complex reasoning
claude --model haiku # For quick tasks
```

## Common Patterns

### Daily Development

```bash
# Start your day
cd project
claude

# Review changes
"what did I change yesterday?"

# Continue work
"finish implementing the user profile feature"

# Commit
"commit my changes with a good message"
```

### Code Review

```bash
# Review PR
git diff main | claude -p "review these changes for issues"

# Address feedback
claude
"fix the security issue in the authentication handler"
```

### Bug Fixing

```bash
# Start with error
claude "fix: TypeError: Cannot read property 'name' of undefined in dashboard"

# Debug
"show me where this error occurs"
"explain why this is happening"
"fix it and add tests to prevent regression"
```

## Troubleshooting

### Command Not Found

```bash
# Verify installation
npm list -g @anthropic-ai/claude-code

# Check PATH
echo $PATH | grep npm

# Reinstall if needed
npm install -g @anthropic-ai/claude-code
```

### Authentication Issues

```bash
# Log out and back in
claude
/logout
/login

# Or reset auth
rm -rf ~/.config/claude-code/auth.json
claude
/login
```

### Performance Issues

```bash
# Clear conversation
/clear

# Compact context
/compact

# Use more specific queries
```

## Getting Help

**Within Claude Code:**
```
/help           # Show all commands
/bug            # Report an issue
/doctor         # Check installation
```

**External Resources:**
- Documentation: https://docs.claude.com/en/docs/claude-code
- GitHub: https://github.com/anthropics/claude-code
- Issues: https://github.com/anthropics/claude-code/issues

## Summary

**Installation:** ~30 seconds
```bash
npm install -g @anthropic-ai/claude-code
claude
/login
```

**First task:** Ask about your project
```
"what does this project do?"
```

**Make changes:** Be specific
```
"add input validation to the signup form"
```

**Commit:** Use natural language
```
"commit my changes"
```

That's it! You're ready to use Claude Code for development.
