---
description: "Generate custom themed spinner verbs. Use /spinnerverbs:create [theme] --parody --cynic --scope [local|project|user]"
allowed-tools: ["Write", "Read", "Bash"]
arguments:
  - name: "theme"
    description: "Theme description (e.g., pirate, chef, corporate, 'real estate appraisal')"
  - name: "parody"
    description: "Add Claude Code / AI / LLM parody references"
  - name: "cynic"
    description: "Add pessimistic / world-weary / Murphy's Law twist"
  - name: "scope"
    description: "Where to save: local, project, or user (default: user)"
  - name: "merge"
    description: "When using both --parody and --cynic, merge both variant styles"
  - name: "combine"
    description: "When using both modifiers, create combined phrases with both styles in each"
---

# Create Themed Spinner Verbs

Generate custom spinner verbs based on a theme description.

**Arguments received:** $ARGUMENTS

## Parse Arguments

Extract from arguments:
- **Theme:** The descriptive theme (everything except flags)
- **--parody:** If present, add Claude Code / AI parody references
- **--cynic:** If present, add pessimistic / world-weary twist
- **--scope:** `local` | `project` | `user` (default: `user`)
- **--merge:** When both --parody and --cynic present, include phrases from both styles
- **--combine:** When both --parody and --cynic present, each phrase has both styles

## Spinner Verbs Schema

```json
{
  "spinnerVerbs": {
    "mode": "replace",
    "verbs": ["phrase1", "phrase2", ...]
  }
}
```

## Generation Instructions

1. **Generate 20-30 themed phrases** based on the theme description
2. **Base phrases** should capture the theme's vocabulary, tone, and iconic references
3. **Apply modifiers if requested:**

### --parody Mode (Claude Code / AI Parody)

Reference `$CLAUDE_PLUGIN_ROOT/skills/theme-creation/references/claude-code-terms.md` for terms to incorporate:
- Context windows, tokens, rate limits
- Sub-agents, MCP servers, tool calls
- Hallucinating, temperature, prompts
- Slash commands, skills, hooks
- Git operations, refactoring, debugging

**Examples:**
- "Beam me up" → "Beam me up, the context window is full"
- "Engage" → "Spawning sub-agent. Engage"
- "Winter is coming" → "Winter is coming... along with more tokens"

### --cynic Mode (Pessimistic / World-Weary)

Add Murphy's Law humor, resigned sighs, and world-weary observations:
- Add "(sigh)", "...again", "...unfortunately" suffixes
- Reference things going wrong, taking longer, being harder
- Self-deprecating about the task at hand

**Examples:**
- "Measuring GFA" → "Measuring GFA for the third time (sigh)"
- "Negotiating" → "Sweating bullets while negotiating"
- "This is the way" → "This is the way... unfortunately"

### Combined Mode (--parody --cynic)

If `--merge`: Include separate phrases from both styles (40-50 total)
If `--combine`: Each phrase incorporates BOTH tech parody AND cynicism

**Combined examples:**
- "The sub-agent is dead, Jim... again"
- "Resistance is futile. So is debugging."
- "Hallucinating with Vulcan precision (sigh)"

## Output Location

Determine save location based on `--scope`:
- `local`: `.claude/settings.local.json`
- `project`: `.claude/settings.json`
- `user` (default): `~/.claude/settings.json`

## Execution Steps

1. Parse theme and flags from arguments
2. Generate 20-30 base themed phrases
3. If `--parody`: Transform/add phrases with Claude Code references
4. If `--cynic`: Transform/add phrases with pessimistic twist
5. If both --parody and --cynic with `--merge`: Combine both sets
6. If both --parody and --cynic with `--combine`: Create dual-style phrases
7. Write JSON to appropriate settings.json location
8. Display preview of generated verbs

## Example Output

For `/spinnerverbs:create pirate --parody`:

```json
{
  "spinnerVerbs": {
    "mode": "replace",
    "verbs": [
      "Avast! Scanning the horizon",
      "Plundering the codebase",
      "Hoisting the mainsail",
      "Charting a course through tokens",
      "Swabbing the context window",
      "Walking the plank... of deprecated APIs",
      "Shiver me sub-agents!",
      "Treasure hunting in the repo",
      "Yo ho ho and a bottle of JSON",
      "Spawning crew member. Energize, matey!"
    ]
  }
}
```

## Confirm with User

After generating, show the user the full list and ask:
- ✅ Apply these verbs
- 🔄 Regenerate with different style
- ✏️ Edit specific phrases
- ❌ Cancel
