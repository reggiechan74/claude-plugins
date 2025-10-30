# Claude Code Metaskill Plugin

Comprehensive documentation Skills for all Claude Code features. This plugin automatically loads relevant documentation when you mention Claude Code topics, eliminating the need to manually run `/prime` for every feature.

## What It Does

This plugin contains 21 Skills covering every aspect of Claude Code:

- **slash-commands** - Creating custom commands
- **sub-agents** - Task delegation and specialized agents
- **hooks** - Event automation and workflows
- **plugins** - Plugin creation and marketplaces
- **agent-skills** - Creating Skills for Claude
- **mcp** - Model Context Protocol integration
- **memory** - CLAUDE.md files and memory management
- **configuration** - Settings and permissions
- **git-workflow** - Commits, PRs, and git safety
- **ci-cd** - GitHub Actions and GitLab CI/CD
- **output-styles** - Output customization (deprecated)
- **headless** - Non-interactive automation mode
- **troubleshooting** - Common issues and solutions
- **deployment** - AWS Bedrock and Google Vertex AI
- **administration** - Monitoring, costs, and analytics
- **ide** - VS Code and JetBrains integration
- **cli-reference** - Complete CLI command reference
- **overview** - Claude Code capabilities and features
- **getting-started** - Quickstart and installation
- **interactive-mode** - REPL features and shortcuts
- **checkpointing** - Rewinding and state management

## How It Works

When you mention topics like "slash commands," "hooks," or "subagents," Claude automatically loads the relevant Skill and provides comprehensive documentation. No need to search docs or remember URLs!

**Examples:**
- "How do I create a slash command?" → loads slash-commands Skill
- "Tell me about hooks" → loads hooks Skill
- "I want to use AWS Bedrock" → loads deployment Skill
- "What keyboard shortcuts are available?" → loads interactive-mode Skill

## Installation

### From Marketplace

```bash
# Add the marketplace
/plugin marketplace add reggiechan74/claude-plugins

# Install the plugin
/plugin install claude-code-metaskill@reggiechan74
```

### Local Development (Testing)

```bash
/plugin marketplace add ./claude-code-metaskill
/plugin install claude-code-metaskill@local-dev
```

### Auto-install in Projects

Add to `.claude/settings.json`:

```json
{
  "extraKnownMarketplaces": {
    "reggiechan74-tools": {
      "source": {
        "source": "github",
        "repo": "reggiechan74/claude-plugins"
      }
    }
  }
}
```

## Benefits

✅ **No more manual doc lookups** - Documentation loads automatically
✅ **Context-aware** - Only loads relevant docs when needed
✅ **Always available** - Works across all your projects
✅ **Comprehensive** - Covers every Claude Code feature
✅ **Up-to-date** - Based on official documentation
✅ **Progressive disclosure** - Minimal context usage

## Skills Included

### Core Features (6 Skills)
- slash-commands
- sub-agents
- hooks
- plugins
- agent-skills
- mcp

### Configuration & Administration (3 Skills)
- memory
- configuration
- administration

### Development Workflows (2 Skills)
- git-workflow
- ci-cd

### IDE & CLI (3 Skills)
- ide
- cli-reference
- interactive-mode

### Deployment (1 Skill)
- deployment

### Getting Started (3 Skills)
- overview
- getting-started
- troubleshooting

### Advanced Features (3 Skills)
- headless
- checkpointing
- output-styles

## Usage Tips

**Be specific in your questions:**
- ✅ "How do I create a custom slash command?"
- ❌ "Tell me about commands"

**Mention the feature by name:**
- ✅ "I want to set up hooks for auto-formatting"
- ❌ "How do I automate things?"

**Ask about specific topics:**
- ✅ "How does checkpointing work?"
- ❌ "Tell me about features"

## Development

### Structure

```
claude-code-metaskill/
├── .claude-plugin/
│   ├── plugin.json          # Plugin metadata
│   └── marketplace.json     # Marketplace config
└── skills/
    ├── slash-commands/
    │   └── SKILL.md
    ├── sub-agents/
    │   └── SKILL.md
    └── ... (21 Skills total)
```

### Adding New Skills

1. Create directory: `skills/new-skill/`
2. Add `SKILL.md` with frontmatter:
```yaml
---
name: new-skill
description: What this skill does and when to use it
---
```
3. Write documentation content
4. Test locally before publishing

### Updating Skills

1. Edit the relevant `SKILL.md` file
2. Test changes locally
3. Commit and push to repository
4. Users get updates automatically

## Publishing to GitHub

1. Create new repository on GitHub
2. Move plugin there:
```bash
cd claude-code-metaskill
git init
git add .
git commit -m "Initial commit: Claude Code metaskill plugin"
git remote add origin git@github.com:your-username/claude-code-metaskill.git
git push -u origin main
```

3. Update installation instructions with your repo URL

## Contributing

Contributions welcome! To improve a Skill:

1. Fork the repository
2. Edit the relevant `SKILL.md` file
3. Test your changes locally
4. Submit a pull request

## Version History

### v1.0.0 (Current)
- Initial release
- 21 comprehensive Skills
- Complete Claude Code documentation coverage
- All features from official docs included

## License

MIT License - Feel free to use and modify

## Credits

Documentation sourced from official Claude Code documentation at https://docs.claude.com/en/docs/claude-code

Plugin created to make Claude Code documentation instantly accessible without manual lookups.

## Support

For issues or questions:
- Check the troubleshooting Skill: `"How do I troubleshoot Claude Code?"`
- Review official docs: https://docs.claude.com/en/docs/claude-code
- Open an issue on GitHub

## Acknowledgments

Thanks to Anthropic for creating Claude Code and providing comprehensive documentation.
