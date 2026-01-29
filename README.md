# Reggie Chan's Claude Code Plugins

A marketplace of Claude Code plugins for enhanced development workflows.

## Installation

Add this marketplace to Claude Code:

```bash
/plugin marketplace add reggiechan74/cc-plugins
```

## Available Plugins

### spinnerverbs

Generate and apply themed spinner verbs for Claude Code status messages.

**Install:**
```bash
/plugin install spinnerverbs@reggiechan74
```

**Features:**
- Pre-built themes: Star Trek, Game of Thrones, Mandalorian
- Custom theme generation from any description
- Style modifiers: `--parody` (AI/coding humor) and `--cynic` (pessimistic twist)
- Flexible scoping: local, project, or user-level

**Commands:**
- `/spinnerverbs:create [theme]` - Generate custom themed spinner verbs
- `/spinnerverbs:apply [template]` - Apply a pre-built theme

## Usage

After adding the marketplace, you can browse and install plugins using:

```bash
/plugin
```

Or install directly by name as shown above.

## Contributing

Found an issue or have a suggestion? Please open an issue in the repository.

## License

Each plugin has its own license. See individual plugin directories for details.
