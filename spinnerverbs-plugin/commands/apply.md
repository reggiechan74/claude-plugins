---
description: "Apply pre-built spinner verb themes. Use /spinnerverbs:apply [startrek|gameofthrones|mandalorian] --parody --cynic --scope [local|project|user]"
allowed-tools: ["Write", "Read"]
arguments:
  - name: "template"
    description: "Template name: startrek, gameofthrones, or mandalorian"
  - name: "parody"
    description: "Use the parody variant (Claude Code / AI references)"
  - name: "cynic"
    description: "Use the cynic variant (pessimistic / world-weary)"
  - name: "scope"
    description: "Where to save: local, project, or user (default: user)"
  - name: "merge"
    description: "Merge base + selected variant(s) together"
  - name: "replace"
    description: "Replace with selected variant only (default behavior)"
---

# Apply Spinner Verb Template

Apply a pre-built theme template to your Claude Code settings.

**Arguments received:** $ARGUMENTS

## Available Templates

| Template | Description |
|----------|-------------|
| `startrek` | Star Trek quotes from TOS, TNG, DS9, Voyager |
| `gameofthrones` | Quotes from Westeros and the Seven Kingdoms |
| `mandalorian` | This is the way... Mandalorian quotes |

## Parse Arguments

Extract from arguments:
- **Template name:** `startrek`, `gameofthrones`, or `mandalorian`
- **--parody:** Use the parody variant
- **--cynic:** Use the cynic variant
- **--scope:** `local` | `project` | `user` (default: `user`)
- **--merge:** Combine base + variant(s)
- **--replace:** Use variant only (default)

## Template Files

Templates are located at `$CLAUDE_PLUGIN_ROOT/templates/[theme]/`:

```
templates/
├── startrek/
│   ├── base.json      # Classic Star Trek quotes
│   ├── parody.json    # With Claude Code parody
│   └── cynic.json     # With pessimistic twist
├── gameofthrones/
│   ├── base.json
│   ├── parody.json
│   └── cynic.json
└── mandalorian/
    ├── base.json
    ├── parody.json
    └── cynic.json
```

## Execution Steps

1. **Validate template name** - Must be one of: startrek, gameofthrones, mandalorian
2. **Read template file(s)** based on flags:
   - No flags: Read `base.json`
   - `--parody`: Read `parody.json`
   - `--cynic`: Read `cynic.json`
   - `--parody --cynic`: Read both variant files
3. **Combine verbs** based on mode:
   - `--replace` (default): Use only the selected variant(s)
   - `--merge`: Combine base.json + selected variant(s)
4. **Determine output location** based on `--scope`:
   - `local`: `.claude/settings.local.json`
   - `project`: `.claude/settings.json`
   - `user` (default): `~/.claude/settings.json`
5. **Write settings.json** with spinnerVerbs configuration
6. **Confirm** what was applied

## Output Format

```json
{
  "spinnerVerbs": {
    "mode": "replace",
    "verbs": [
      "Phrase 1",
      "Phrase 2",
      "..."
    ]
  }
}
```

## Examples

### Basic Apply
```
/spinnerverbs:apply startrek
```
Applies Star Trek base.json to ~/.claude/settings.json

### Apply with Parody
```
/spinnerverbs:apply mandalorian --parody
```
Applies Mandalorian parody.json (Claude Code parody version)

### Apply with Cynic + Merge
```
/spinnerverbs:apply gameofthrones --cynic --merge
```
Combines base.json + cynic.json for Game of Thrones

### Apply Both Variants
```
/spinnerverbs:apply startrek --parody --cynic
```
Combines parody.json + cynic.json (no base unless --merge)

### Full Merge to Project
```
/spinnerverbs:apply startrek --parody --cynic --merge --scope project
```
Combines base + parody + cynic, saves to .claude/settings.json

## Confirmation Output

After applying, show:
```
✅ Applied [template] spinner verbs to [location]

Variant: [base|parody|cynic|parody+cynic]
Mode: [replace|merge]
Phrases: [count]

Preview:
- "Phrase 1"
- "Phrase 2"
- "Phrase 3"
...
```
