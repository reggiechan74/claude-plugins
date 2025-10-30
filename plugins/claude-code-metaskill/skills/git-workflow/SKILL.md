---
name: git-workflow
description: How Claude Code handles git commits and pull requests, including git safety protocols and best practices. Use when user asks about creating commits, making PRs, git workflows, or git safety.
---

# Git Workflow in Claude Code

## Overview

Claude Code provides intelligent git workflow automation with built-in safety protocols. It can create commits, generate pull requests, and manage git operations while following best practices and security guidelines.

## Git Safety Protocol

**NEVER:**
- Update the git config
- Run destructive/irreversible git commands (like push --force, hard reset, etc.) unless explicitly requested
- Skip hooks (--no-verify, --no-gpg-sign, etc.) unless explicitly requested
- Run force push to main/master (warn the user if they request it)
- Commit changes unless the user explicitly asks

**Avoid git commit --amend** - ONLY use --amend when either:
1. User explicitly requested amend OR
2. Adding edits from pre-commit hook

**Before amending:**
- ALWAYS check authorship: `git log -1 --format='%an %ae'`
- NEVER amend other developers' commits

## Creating Commits

### When to Commit
Only create commits when the user explicitly requests them. It is VERY IMPORTANT to only commit when explicitly asked, otherwise the user will feel that you are being too proactive.

### Commit Workflow

**Step 1 - Gather Information (in parallel):**
Run these bash commands simultaneously:
- `git status` - see all untracked files
- `git diff` - see both staged and unstaged changes
- `git log` - see recent commit messages to follow the repository's commit message style

**Step 2 - Analyze and Draft:**
- Summarize the nature of changes (new feature, enhancement, bug fix, refactoring, test, docs, etc.)
- Do not commit files that likely contain secrets (.env, credentials.json, etc.)
- Warn the user if they specifically request to commit those files
- Draft a concise (1-2 sentences) commit message that focuses on the "why" rather than the "what"
- Ensure it accurately reflects the changes and their purpose

**Step 3 - Execute Commit (sequentially):**
Add relevant untracked files to the staging area, then create the commit with a message ending with:

```
🤖 Generated with [Claude Code](https://claude.com/claude-code)

Co-Authored-By: Claude <noreply@anthropic.com>
```

Then run `git status` after the commit completes to verify success.

**Step 4 - Handle Pre-commit Hooks:**
If the commit fails due to pre-commit hook changes, retry ONCE. If it succeeds but files were modified by the hook, verify it's safe to amend:
- Check authorship: `git log -1 --format='%an %ae'`
- Check not pushed: `git status` shows "Your branch is ahead"
- If both true: amend your commit
- Otherwise: create NEW commit (never amend other developers' commits)

### Commit Message Format

Always pass the commit message via HEREDOC for good formatting:

```bash
git commit -m "$(cat <<'EOF'
Commit message here.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

Co-Authored-By: Claude <noreply@anthropic.com>
EOF
)"
```

### Important Notes
- NEVER run additional commands to read or explore code, besides git bash commands
- NEVER use the TodoWrite or Task tools during git commits
- DO NOT push to the remote repository unless the user explicitly asks
- NEVER use git commands with the -i flag (like git rebase -i or git add -i) since they require interactive input
- If there are no changes to commit, do not create an empty commit

## Creating Pull Requests

### When to Create PRs
Use the `gh` command via the Bash tool for ALL GitHub-related tasks including working with issues, pull requests, checks, and releases.

### Pull Request Workflow

**Step 1 - Understand Branch State (in parallel):**
Run these bash commands simultaneously:
- `git status` - see all untracked files
- `git diff` - see both staged and unstaged changes
- Check if the current branch tracks a remote branch and is up to date
- `git log` and `git diff [base-branch]...HEAD` - understand the full commit history from when the branch diverged

**Step 2 - Analyze All Commits:**
Analyze all changes that will be included in the pull request, making sure to look at all relevant commits (NOT just the latest commit, but ALL commits that will be included in the pull request).

Draft a pull request summary covering:
- What changed
- Why it changed
- How to test it

**Step 3 - Create PR (in parallel if needed):**
- Create new branch if needed
- Push to remote with -u flag if needed
- Create PR using `gh pr create` with the format below

Use a HEREDOC to pass the body for correct formatting:

```bash
gh pr create --title "the pr title" --body "$(cat <<'EOF'
## Summary
<1-3 bullet points>

## Test plan
[Bulleted markdown checklist of TODOs for testing the pull request...]

🤖 Generated with [Claude Code](https://claude.com/claude-code)
EOF
)"
```

### Important Notes
- DO NOT use the TodoWrite or Task tools
- Return the PR URL when done
- All changes flow through PRs for human review
- Branch protection rules still apply

## Using GitHub CLI (gh)

The `gh` command provides comprehensive GitHub integration:

**View PR comments:**
```bash
gh api repos/foo/bar/pulls/123/comments
```

**Work with issues:**
```bash
gh issue list
gh issue create --title "Bug report" --body "Description"
```

**Check PR status:**
```bash
gh pr status
gh pr view 123
```

## Best Practices

1. **Always check git status** before and after operations
2. **Review diffs carefully** before committing
3. **Follow repository conventions** for commit messages
4. **Never commit secrets** or sensitive files
5. **Verify branch state** before creating PRs
6. **Include all relevant commits** in PR summaries
7. **Use descriptive titles** and comprehensive descriptions
8. **Test before pushing** when possible
9. **Respect git hooks** and handle their feedback appropriately
10. **Ask for clarification** if unsure about destructive operations
