---
name: theme-creation
description: "Use when creating custom spinner verb themes, especially when applying --parody (Claude Code parody) or --cynic (pessimistic) modifiers to themed phrases"
---

# Spinner Verb Theme Creation

Guide for creating effective, entertaining spinner verb themes with optional humor modifiers.

## Spinner Verb Basics

Spinner verbs are status messages displayed while Claude Code works. They appear randomly from a flat array of strings. Good spinner verbs are:

- **Short to medium length** (1-8 words ideal)
- **Action-oriented** or **situational**
- **Thematically consistent**
- **Entertaining** without being distracting

## Theme Creation Process

### 1. Identify Theme Elements

For any theme, identify:
- **Iconic catchphrases** - Well-known quotes
- **Character-specific lines** - Memorable character dialogue
- **Action verbs** - What characters do in that universe
- **Situational phrases** - Common scenarios
- **Technical jargon** - Universe-specific terminology

### 2. Generate Base Phrases (20-30)

Create a mix of:
- Direct quotes (most recognizable)
- Paraphrased actions ("Engaging warp drive")
- Situational states ("Red alert!")
- Character voice lines ("I have spoken")

### 3. Apply Modifiers

See `references/claude-code-terms.md` for `--parody` mode vocabulary.
See `references/cynic-patterns.md` for `--cynic` mode patterns.

## --parody Mode: Claude Code Parody

Transform phrases by incorporating AI/LLM/Claude Code references:

| Original | With --parody |
|----------|---------------|
| "Beam me up" | "Beam me up, the context window is full" |
| "Engage!" | "Spawning sub-agent. Engage!" |
| "He's dead, Jim" | "The sub-agent is dead, Jim" |
| "I have spoken" | "I have spoken... to the MCP server" |
| "Winter is coming" | "Winter is coming... along with more tokens" |

**Key substitutions:**
- Ship/vessel → context window, token buffer
- Crew/team → sub-agents, tools
- Communications → MCP servers, API calls
- Weapons → tool calls, parallel execution
- Shields → rate limits, guardrails
- Engineering → prompt engineering, refactoring

## --cynic Mode: Pessimistic Twist

Transform phrases by adding world-weary, Murphy's Law humor:

| Original | With --cynic |
|----------|--------------|
| "Engage!" | "Engaging... reluctantly" |
| "This is the way" | "This is the way... unfortunately" |
| "I shall return" | "I shall return... to fix this again" |
| "Winter is coming" | "Winter is coming... along with more bugs" |
| "Measuring GFA" | "Measuring GFA for the third time (sigh)" |

**Cynic patterns:**
- Add "(sigh)" suffix
- Add "...again" or "...unfortunately"
- Reference things taking longer than expected
- Self-deprecating about the task
- Murphy's Law observations
- Resigned acceptance

## Combined Mode (--parody --cynic)

When both modifiers are used:

**Merge mode:** Include phrases from both styles separately (40-50 total)

**Combine mode:** Each phrase has BOTH tech parody AND cynicism:
- "The sub-agent is dead, Jim... again"
- "Resistance is futile. So is debugging."
- "Spawning sub-agent... it'll probably timeout"
- "Context window at 20%... as usual"

## Quality Checklist

Before finalizing a theme:
- [ ] 20-30 phrases minimum
- [ ] Mix of quotes, actions, and situations
- [ ] Recognizable to fans of the theme
- [ ] Humor is clever, not forced
- [ ] No offensive or inappropriate content
- [ ] Varied length (some short, some medium)
- [ ] Grammatically correct
