---
name: ci-cd
description: How to use Claude Code with GitHub Actions and GitLab CI/CD for automated workflows. Use when user asks about CI/CD integration, GitHub Actions, GitLab pipelines, or automated development workflows.
---

# Claude Code CI/CD Integration

## Overview

Claude Code integrates with CI/CD platforms to enable AI-powered automation within development workflows. By mentioning `@claude` in pull requests, merge requests, or issues, you can leverage Claude to analyze code, create PRs/MRs, implement features, and fix bugs while adhering to project standards.

## GitHub Actions

### Key Capabilities

**Core Features:**
- Instant PR creation with complete code changes from descriptions
- Automated issue-to-code implementation
- Adherence to project guidelines via `CLAUDE.md` files
- Secure execution on GitHub runners

### Setup Methods

**Quick Installation:**
Run `/install-github-app` in Claude Code for guided setup and secret configuration. This approach is limited to direct Claude API users.

**Manual Setup Requirements:**
1. Install the Claude GitHub app with read/write permissions for Contents, Issues, and Pull Requests
2. Add `ANTHROPIC_API_KEY` to repository secrets
3. Copy the workflow file from official examples into `.github/workflows/`

### Configuration

**Example Workflow:**
```yaml
- uses: anthropics/claude-code-action@v1
  with:
    prompt: "Your instructions"
    anthropic_api_key: ${{ secrets.ANTHROPIC_API_KEY }}
    claude_args: "--max-turns 5"
```

**Version Migration (v1.0):**
- Replace `mode` configuration (now auto-detected)
- Change `direct_prompt` to `prompt`
- Move CLI options into `claude_args` parameter

### Common Use Cases

**In comments:**
```
@claude implement this feature
@claude fix the TypeError in the dashboard
@claude how should I handle authentication?
```

### Best Practices

**Project Guidelines:** Create a `CLAUDE.md` file defining coding standards and review criteria that Claude will follow.

**Security:** Always use GitHub Secrets rather than hardcoding credentials. Store API keys as `ANTHROPIC_API_KEY`.

**Cost Management:** Configure `--max-turns` limits and use specific commands to minimize unnecessary API calls.

### Enterprise Deployment

AWS Bedrock and Google Vertex AI integration requires:
- OIDC configuration for authentication
- Service account setup with appropriate permissions
- Repository secrets for credentials

Both approaches eliminate need for static credentials through temporary token rotation.

### Troubleshooting

**No Response:** Verify GitHub app installation, workflow enablement, and use of `@claude` (not `/claude`).

**CI Not Running:** Confirm app usage instead of default Actions user and verify webhook trigger configuration.

**Authentication Issues:** Validate API key permissions and confirm secret naming in workflows.

## GitLab CI/CD

### Overview
Claude Code integrates with GitLab CI/CD to enable AI-powered development workflows. The integration is currently in beta and maintained by GitLab.

### Key Capabilities
- **Automated MR Creation**: Describe what you need, and Claude proposes a complete MR with changes and explanation
- **Issue-to-Code**: Transform issue descriptions into working implementations
- **Code Review & Iteration**: Respond to follow-up comments to refine proposed changes
- **Bug Fixes**: Identify and resolve issues identified through tests or comments

### Quick Setup

**1. Add CI/CD Variable:**
Store `ANTHROPIC_API_KEY` as a masked variable in **Settings → CI/CD → Variables**

**2. Configure .gitlab-ci.yml:**
Add a Claude job stage that:
- Uses Node.js Alpine image
- Installs Claude Code CLI via npm
- Executes Claude with appropriate prompts
- Allows tools: Bash, Read, Edit, Write, and mcp__gitlab

**3. Trigger Methods:**
- Manual pipeline runs
- Merge request events
- Web/API triggers when comments mention `@claude`

### Provider Options

**Claude API (SaaS):** Use `ANTHROPIC_API_KEY`

**AWS Bedrock:** Configure OIDC authentication with:
- `AWS_ROLE_TO_ASSUME` variable
- `AWS_REGION` variable
- IAM role with Bedrock permissions

**Google Vertex AI:** Set up Workload Identity Federation with:
- `GCP_WORKLOAD_IDENTITY_PROVIDER`
- `GCP_SERVICE_ACCOUNT`
- `CLOUD_ML_REGION`

### Best Practices

- **CLAUDE.md**: Define project conventions and coding standards for Claude to follow
- **Security**: Never commit credentials; use masked CI/CD variables and OIDC where possible
- **Performance**: Keep guidelines concise; provide clear issue descriptions to minimize iterations
- **Governance**: All changes flow through MRs for review; branch protection rules apply

### Cost Considerations
- GitLab runner compute minutes consumed by Claude jobs
- Anthropic API token usage (varies by task complexity and codebase size)
- Optimize by using specific commands and setting appropriate timeouts

### Use Case Examples

**Convert Issues to MRs:** Comment `@claude implement this feature based on the issue description`

**Get Implementation Help:** In MR discussions: `@claude suggest a concrete approach to [task]`

**Fix Bugs:** `@claude fix the [error type] in [component]`

## General CI/CD Best Practices

1. **Define clear guidelines** in `CLAUDE.md` for consistent behavior
2. **Use secrets management** for all credentials
3. **Set cost controls** with turn limits and specific prompts
4. **Review all changes** - Claude creates PRs/MRs for human approval
5. **Monitor usage** to track API consumption and optimize workflows
6. **Test in non-production** environments first
