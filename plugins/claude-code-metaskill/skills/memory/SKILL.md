---
name: memory
description: How to use CLAUDE.md memory files for persistent instructions across sessions. Use when user asks about CLAUDE.md, memory files, project instructions, or persistent context.
---

# Claude Code Memory (CLAUDE.md)

## Overview
Claude Code maintains persistent memories across sessions using CLAUDE.md files organized in a hierarchical structure with four memory locations.

## Memory Hierarchy

**Enterprise Policy** (highest priority)
- macOS: `/Library/Application Support/ClaudeCode/CLAUDE.md`
- Linux: `/etc/claude-code/CLAUDE.md`
- Windows: `C:\ProgramData\ClaudeCode\CLAUDE.md`
- Organization-wide instructions for all users

**Project Memory**
- Location: `./CLAUDE.md` or `./.claude/CLAUDE.md`
- Purpose: Team-shared instructions for the project
- Shared via source control

**User Memory**
- Location: `~/.claude/CLAUDE.md`
- Personal preferences applying across all projects

**Project Memory (Local)** — Deprecated
- `./CLAUDE.local.md` is now superseded by imports

## CLAUDE.md Imports Feature

Files support `@path/to/import` syntax for including additional content. Examples:

```markdown
See @README for project overview and @package.json for available npm commands
```

Import paths can be relative or absolute. Practical use case for individual preferences:

```markdown
Individual Preferences - @~/.claude/my-project-instructions.md
```

Imports ignore markdown code spans and support recursive inclusion up to 5 levels deep.

## Core Usage Methods

### Quick Addition with `#` Shortcut
Start input with `#` followed by your memory. The system prompts you to select the target memory file.

### Direct Editing
Use `/memory` slash command to open memory files in your system editor for extensive modifications.

### Initialization
Run `/init` to bootstrap a CLAUDE.md file with project-specific information.

## Best Practices

**Be Specific**: "Use 2-space indentation" outperforms "Format code properly."

**Structure**: Format memories as bullet points under descriptive markdown headings.

**Review Periodically**: Update memories as projects evolve.

**Ideal Memory Content**:
- Frequently-used build commands
- Code style preferences
- Naming conventions
- Architectural patterns specific to your project
- Testing requirements
- Deployment procedures
- Code review criteria

## Example CLAUDE.md

```markdown
# Project Instructions

## Code Style
- Use 2-space indentation
- Prefer functional components in React
- Use TypeScript strict mode
- Follow ESLint rules without exceptions

## Build Commands
- `npm run dev` - Start development server
- `npm run test` - Run test suite
- `npm run build` - Production build

## Architecture
- API routes in `src/api/`
- Components in `src/components/`
- Utilities in `src/utils/`
- Follow feature-based folder structure

## Testing
- Write unit tests for all utilities
- Integration tests for API routes
- Use React Testing Library for components

## External Resources
See @README.md for project overview
See @CONTRIBUTING.md for contribution guidelines
```

## Working with Team Memories

**Project-level CLAUDE.md**:
- Commit to source control
- Share coding standards across team
- Define common workflows
- Document project-specific conventions

**Local overrides**:
- Use imports to extend project memory
- Add personal preferences without affecting team
- Reference local configuration files

## Memory Hierarchy in Practice

When Claude processes a request, it reads all applicable memory files in order of precedence:
1. Enterprise policy (if configured)
2. Project memory (team-shared)
3. User memory (personal preferences)

Settings in higher-priority files take precedence over lower-priority ones.

## Common Use Cases

**Onboarding**: New team members get instant context from project CLAUDE.md

**Consistency**: Team maintains consistent code style through shared memory

**Personalization**: Individual developers add personal preferences via user memory

**Security**: Enterprise policies enforce security requirements globally

**Documentation**: Import existing project docs to provide context

## Tips

- Keep memories focused and actionable
- Use headings to organize different types of instructions
- Update memories when conventions change
- Leverage imports to avoid duplication
- Review memories periodically for relevance
- Use specific examples rather than vague guidelines
