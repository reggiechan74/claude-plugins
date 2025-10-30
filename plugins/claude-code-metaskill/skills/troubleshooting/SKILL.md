---
name: troubleshooting
description: Common issues and solutions for Claude Code installation, authentication, performance, and IDE integration. Use when user encounters errors, problems, or asks about debugging.
---

# Claude Code Troubleshooting

## Installation Issues

### Windows WSL Problems

**OS/platform detection issues:**
May require running `npm config set os linux` before installation.

**Node.js path conflicts:**
WSL may use Windows npm instead of Linux versions.
- Check with `which npm` and `which node`
- Identify whether Linux or Windows paths are active
- nvm version conflicts can be resolved by ensuring nvm loads in shell configuration files

**Recommended Solution:**
Use the native Claude Code installer as an alternative to npm:
```bash
curl -fsSL https://claude.ai/install.sh | bash
```

### Linux/Mac Permission Errors

**Native installer (recommended):**
```bash
curl -fsSL https://claude.ai/install.sh | bash
```

**Migration from npm:**
Migrate to local installation to avoid future permission issues:
```bash
claude migrate-installer
```

## Authentication & Permissions

### Reset Authentication

Run `/logout` and restart Claude Code to reset authentication.

For persistent issues, remove stored auth data:
```bash
rm -rf ~/.config/claude-code/auth.json
```

### Manage Permissions

Use `/permissions` to allow specific tools without repeated approval prompts.

## Performance Issues

### Reduce Context Size

Use `/compact` regularly to reduce context size for large codebases.

### Cancel Unresponsive Operations

Press `Ctrl+C` to cancel unresponsive operations.

### Fix Search Functionality

Install system `ripgrep` to fix search functionality:
```bash
# macOS
brew install ripgrep

# Ubuntu/Debian
sudo apt install ripgrep

# Fedora
sudo dnf install ripgrep
```

## IDE Integration Issues

### JetBrains on WSL2

**Firewall Issues:**
Configure Windows Firewall or enable mirrored networking mode.

Add to `.wslconfig`:
```ini
[wsl2]
networkingMode=mirrored
```

### ESC Key Not Working (JetBrains)

Go to Settings → Tools → Terminal and disable "Move focus to the editor with Escape."

Or delete the "Switch focus to Editor" shortcut.

## Markdown Issues

### Missing Language Tags

Request language tags explicitly:
```
"Add appropriate language tags to all code blocks."
```

### Automatic Formatting

Use formatting hooks for automatic post-processing validation.

## Common Error Messages

### "Command not found: claude"

**Solution:**
1. Verify installation: `npm list -g @anthropic-ai/claude-code`
2. Check PATH includes npm global bin directory
3. Restart terminal
4. Reinstall if necessary

### "Authentication failed"

**Solution:**
1. Run `/logout` then `/login`
2. Verify API key is valid
3. Check network connectivity
4. Remove auth file: `rm -rf ~/.config/claude-code/auth.json`

### "Permission denied"

**Solution:**
1. Check file permissions in project directory
2. Verify user has write access
3. Use `/permissions` to configure allowed operations
4. Check settings.json for overly restrictive deny rules

### "Context too large"

**Solution:**
1. Run `/compact` to reduce context
2. Be more specific in queries
3. Use subagents for isolated tasks
4. Clear conversation with `/clear`

### "Rate limit exceeded"

**Solution:**
1. Wait before retrying
2. Check API usage limits
3. Use `--max-turns` to limit operations
4. Implement delays in automation scripts

## Getting Help

### Built-in Diagnostics

**Report bugs:**
```
/bug
```

**Check installation health:**
```
/doctor
```

### External Resources

- **GitHub Issues**: https://github.com/anthropics/claude-code/issues
- **Documentation**: https://docs.claude.com/en/docs/claude-code
- **Community Support**: Check GitHub Discussions

## Debug Mode

Enable verbose logging:
```bash
claude --debug
claude --verbose
```

View detailed output:
```bash
claude --output-format json
```

## Reinstallation

If all else fails, completely reinstall:

```bash
# Uninstall
npm uninstall -g @anthropic-ai/claude-code

# Clear cache
rm -rf ~/.config/claude-code
rm -rf ~/.claude

# Reinstall
npm install -g @anthropic-ai/claude-code

# Or use native installer
curl -fsSL https://claude.ai/install.sh | bash
```

## Prevention Tips

1. Keep Claude Code updated: `claude update`
2. Regularly run `/compact` for large projects
3. Use specific queries rather than vague requests
4. Configure permissions appropriately
5. Monitor API usage and costs
6. Use version control for important changes
7. Enable checkpointing for easy recovery
