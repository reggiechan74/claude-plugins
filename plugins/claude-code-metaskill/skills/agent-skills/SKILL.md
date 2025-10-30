---
name: agent-skills
description: Complete guide to Agent Skills - modular capabilities that extend Claude's functionality. Use when user asks about creating Skills, Skill structure, progressive disclosure, or custom capabilities for Claude.
---

# Agent Skills

## What Are Agent Skills?

Agent Skills are modular capabilities that extend Claude's functionality by packaging instructions, metadata, and optional resources (scripts, templates) that Claude uses automatically when relevant. They transform Claude from a general-purpose assistant into a domain specialist.

## Key Benefits

- **Specialize Claude**: Tailor capabilities for specific task domains
- **Reduce repetition**: Create once, reuse across conversations automatically
- **Compose capabilities**: Combine multiple Skills for complex workflows

## How Skills Work

Skills operate within Claude's virtual machine environment with filesystem access. They use **progressive disclosure**—loading information in stages as needed rather than consuming context upfront.

### Three Loading Levels

**Level 1 - Metadata (Always loaded)**
YAML frontmatter provides discovery information (~100 tokens per Skill):
```yaml
---
name: pdf-processing
description: Extract text, fill forms, merge documents
---
```

**Level 2 - Instructions (Loaded when triggered)**
Main SKILL.md body contains procedural guidance (under 5k tokens). Claude reads this file only when the Skill matches the user's request.

**Level 3 - Resources (Loaded as needed)**
Additional files (FORMS.md, scripts, reference materials) are accessed only when referenced—effectively unlimited content without token penalty.

## Where Skills Are Available

- **Claude API**: Pre-built and custom Skills via `skill_id` parameter
- **Claude Code**: Filesystem-based custom Skills in `.claude/skills/`
- **Claude Agent SDK**: Custom Skills through configuration
- **Claude.ai**: Pre-built Skills built-in; custom Skills uploadable by users

## Pre-Built Agent Skills

Anthropic provides ready-to-use Skills:
- **PowerPoint (pptx)**: Create and edit presentations
- **Excel (xlsx)**: Build spreadsheets with data analysis
- **Word (docx)**: Generate formatted documents
- **PDF (pdf)**: Create formatted PDF reports

## Skill Structure Requirements

Every Skill needs a `SKILL.md` file with YAML frontmatter:

```markdown
---
name: your-skill-name
description: What this does and when to use it
---

# Your Skill Name

## Instructions
[Step-by-step guidance]

## Examples
[Concrete usage examples]
```

**Requirements**:
- `name`: Maximum 64 characters, lowercase letters/numbers/hyphens only
- `description`: Non-empty, maximum 1024 characters

## Storage Locations in Claude Code

1. **Personal Skills**: `~/.claude/skills/skill-name/`
2. **Project Skills**: `.claude/skills/skill-name/` (shared via git)
3. **Plugin Skills**: bundled with installed plugins

## Supporting Files

Skills can include additional resources:
- Reference documentation
- Example files
- Script utilities
- Templates

Claude loads these progressively based on context.

## Tool Access Control

Use `allowed-tools` frontmatter to restrict Claude's capabilities when using a Skill:
```yaml
allowed-tools: Read, Grep, Glob
```

This limits tool usage without requiring permission prompts.

### Available Tools

**Tools That Require Permission:**
- **Bash** - Execute shell commands
- **Edit** - Make targeted file edits
- **NotebookEdit** - Modify Jupyter notebook cells
- **SlashCommand** - Run custom slash commands
- **WebFetch** - Fetch content from URLs
- **WebSearch** - Perform web searches
- **Write** - Create or overwrite files

**Tools That Don't Require Permission:**
- **Glob** - Find files by pattern matching
- **Grep** - Search for patterns in files
- **NotebookRead** - Read Jupyter notebooks
- **Read** - Read file contents
- **Task** - Run sub-agents
- **TodoWrite** - Manage task lists

### Example Tool Configurations

**Read-only Skill:**
```yaml
allowed-tools: Read, Grep, Glob
```

**Analysis Skill with web access:**
```yaml
allowed-tools: Read, Grep, Glob, WebFetch
```

**Code generation Skill:**
```yaml
allowed-tools: Read, Glob, Grep, Write, Edit, Bash
```

**Research Skill:**
```yaml
allowed-tools: Read, WebFetch, WebSearch, TodoWrite
```

## Best Practices

### Core Principles

**Conciseness**: Context windows are shared resources. Only include information Claude doesn't already possess. Challenge each piece: "Does Claude really need this?"

**Degrees of Freedom**: Match specificity to task fragility:
- High freedom (text): Multiple valid approaches exist
- Medium freedom (pseudocode): Preferred patterns with variation allowed
- Low freedom (exact scripts): Operations are fragile or consistency-critical

**Multi-Model Testing**: Verify Skills work across Haiku, Sonnet, and Opus since effectiveness depends on the underlying model.

### Skill Structure

**Frontmatter Requirements**:
- `name`: 64 characters max, lowercase letters/numbers/hyphens only
- `description`: Non-empty, 1024 characters max, specific about what and when to use

**Naming**: Use gerund form ("processing-pdfs") for clarity and consistency across skill collections.

**Descriptions**: Write in third person, be specific with key terms, explain both functionality and use cases. Avoid vague descriptions like "helps with documents."

### Progressive Disclosure

Keep SKILL.md under 500 lines. Bundle additional content (reference files, examples, domain-specific guides) that Claude loads only when needed. Structure as:
- SKILL.md (overview, navigation)
- Reference files (one level deep from SKILL.md)
- Scripts directory (utility scripts executed, not loaded)

### Workflows & Feedback Loops

- Use checklists for complex multi-step processes
- Implement validation loops: run validator → fix errors → repeat
- Provide templates for strict output requirements
- Include input/output examples for style guidance

### Content Guidelines

Avoid time-sensitive information. Use consistent terminology throughout ("API endpoint" not mixed with "URL" or "route").

### Code & Scripts

- Handle errors explicitly rather than punting to Claude
- Justify all configuration values (no "magic numbers")
- Provide utility scripts for reliability and token efficiency
- Use forward slashes in paths (Unix-style)
- Implement validation steps for critical operations
- Include verifiable intermediate outputs (plan files)

## Evaluation & Testing

Create evaluations before extensive documentation. Build three representative scenarios, establish baselines, write minimal instructions addressing identified gaps, then iterate based on results.

**Iterative Development**: Work with one Claude instance ("Claude A") to design Skills tested by other instances ("Claude B"), observing real behavior and refining accordingly.

## Anti-Patterns

- Windows-style paths (`\`)
- Offering excessive options without defaults
- Deeply nested file references
- Assuming tools are pre-installed
- Time-sensitive constraints in documentation

## Critical Success Factors

**Description Specificity**: Vague descriptions prevent discovery. Include both functionality and usage triggers with concrete terminology users would mention.

**File Paths**: Use Unix-style forward slashes consistently.

**YAML Validation**: Ensure proper opening/closing `---` delimiters and correct indentation (spaces only).

## Team Distribution

Recommended approach: distribute via plugins. Alternative: commit Skills to project repositories; team members automatically access them after pulling changes.

## Security Considerations

Only use Skills from trusted sources. Malicious Skills could direct Claude to misuse tools or expose data. Thoroughly audit all bundled files, scripts, and external resource references before deploying.

## Key Limitations

- **No cross-surface sync**: Skills uploaded to one platform aren't automatically available elsewhere
- **No network access**: Skills cannot make external API calls
- **Pre-installed packages only**: Cannot install new packages during execution
- **Sharing varies**: Claude.ai (individual), API (workspace-wide), Claude Code (personal/project-based)

## Skills vs Slash Commands

**Use slash commands for**: Simple, frequently-used prompts that fit in one file.

**Use Skills for**: Complex capabilities requiring multiple files, scripts, or organizational structure.

Key difference: Commands require explicit invocation; Skills are discovered automatically based on context.

## Checklist for Effective Skills

**Quality**:
- Specific description with use cases
- Under 500 lines
- Proper progressive disclosure
- Consistent terminology

**Code**:
- Error handling
- Justified constants
- Documented scripts
- Forward-slash paths
- Validation workflows

**Testing**:
- Three evaluations created
- Tested across models
- Real-world scenarios validated
