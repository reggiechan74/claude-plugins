---
name: deployment
description: How to deploy Claude Code with Amazon Bedrock, Google Vertex AI, and other cloud providers. Use when user asks about AWS Bedrock, GCP Vertex AI, cloud deployment, or enterprise deployment.
---

# Claude Code Deployment

## Overview

Claude Code supports deployment through multiple providers beyond the direct Claude API, including Amazon Bedrock and Google Vertex AI for enterprise cloud deployment.

## Amazon Bedrock Integration

### Overview
Claude Code integrates with Amazon Bedrock to enable deployment through AWS infrastructure using Claude models available in your AWS account.

### Prerequisites
- Active AWS account with Bedrock access enabled
- Access to desired Claude models (e.g., Claude Sonnet 4.5)
- AWS CLI installed (optional)
- Appropriate IAM permissions

### Setup Process

#### 1. Model Access
Navigate to the Amazon Bedrock console, access Model access settings, and request Claude model availability in your region.

#### 2. AWS Credentials Configuration
Multiple authentication methods are supported:

**AWS CLI:**
```bash
aws configure
```

**Environment variables:**
```bash
export AWS_ACCESS_KEY_ID=your-key
export AWS_SECRET_ACCESS_KEY=your-secret
export AWS_SESSION_TOKEN=your-token  # Optional
```

**SSO profile:**
```bash
aws sso login --profile=<name>
export AWS_PROFILE=your-profile
```

**Bedrock API keys:**
```bash
export AWS_BEARER_TOKEN_BEDROCK=your-token
```

#### 3. Claude Code Configuration
Enable Bedrock integration:
```bash
export CLAUDE_CODE_USE_BEDROCK=1
export AWS_REGION=us-east-1  # Or preferred region
```

Optional override for Haiku region:
```bash
export ANTHROPIC_SMALL_FAST_MODEL_AWS_REGION=us-west-2
```

#### 4. Model Selection
Default models include Claude Sonnet 4.5 and Claude Haiku 4.5.

Customize via:
```bash
export ANTHROPIC_MODEL='model-id'
export ANTHROPIC_SMALL_FAST_MODEL='haiku-model-id'
```

#### 5. Token Configuration
Recommended settings:
```bash
export CLAUDE_CODE_MAX_OUTPUT_TOKENS=4096
export MAX_THINKING_TOKENS=1024
```

### IAM Permissions

Required actions:
- `bedrock:InvokeModel`
- `bedrock:InvokeModelWithResponseStream`
- `bedrock:ListInferenceProfiles`

Example IAM policy:
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "bedrock:InvokeModel",
        "bedrock:InvokeModelWithResponseStream",
        "bedrock:ListInferenceProfiles"
      ],
      "Resource": "*"
    }
  ]
}
```

### Advanced Features

Automatic credential refresh supports corporate identity providers through `awsAuthRefresh` and `awsCredentialExport` configuration options.

### Key Limitations
- Login/logout commands disabled (AWS credentials handle authentication)
- Uses Bedrock's Invoke API, not Converse API

## Google Vertex AI Integration

### Overview
Claude Code integrates with Google Vertex AI to enable deployment through Google Cloud Platform. The service supports both global and regional endpoints for model access.

### Prerequisites
- Active GCP account with billing enabled
- A project with Vertex AI API access
- Google Cloud SDK (`gcloud`) installed
- Appropriate quota allocation in your chosen region

### Setup Process

#### 1. Enable Vertex AI API
Enable the Vertex AI API in your GCP project:
```bash
gcloud config set project YOUR-PROJECT-ID
gcloud services enable aiplatform.googleapis.com
```

#### 2. Request Model Access
Navigate to Vertex AI Model Garden to search for and request access to Claude models like Claude Sonnet 4.5.

**Approval time:** Typically 24-48 hours

#### 3. Configure GCP Credentials
Claude Code uses standard Google Cloud authentication and automatically detects the project ID from environment variables.

```bash
gcloud auth application-default login
```

#### 4. Configure Claude Code
Set environment variables:
```bash
export CLAUDE_CODE_USE_VERTEX=1
export CLOUD_ML_REGION=global  # Or specify regional endpoints
export ANTHROPIC_VERTEX_PROJECT_ID=YOUR-PROJECT-ID
```

#### 5. Model Configuration
Default models include Claude Sonnet 4.5 as the primary model and Claude Haiku 4.5 as the fast model.

Customize through environment variables:
```bash
export ANTHROPIC_MODEL='model-id'
export ANTHROPIC_SMALL_FAST_MODEL='haiku-model-id'
```

### Key Features

**Prompt Caching:**
Automatically supported via `cache_control` flags

**1M Token Context:**
Available in beta for Sonnet 4 and 4.5

**IAM Requirements:**
Assign `roles/aiplatform.user` role for necessary permissions:
```bash
gcloud projects add-iam-policy-binding YOUR-PROJECT-ID \
  --member="user:email@example.com" \
  --role="roles/aiplatform.user"
```

### Troubleshooting

**Quota limitations:**
- Check quota in GCP Console
- Request increases if needed

**Unsupported models in specific regions:**
- Verify model availability in Model Garden
- Switch to supported regional endpoints

**429 rate-limit errors:**
- Implement retry logic
- Request quota increases
- Spread requests across regions

## Comparison: Bedrock vs Vertex AI vs Claude API

| Feature | Claude API | AWS Bedrock | Google Vertex AI |
|---------|-----------|-------------|------------------|
| **Setup Complexity** | Simple | Moderate | Moderate |
| **Authentication** | API key | AWS credentials | GCP credentials |
| **Regional Options** | Global | AWS regions | GCP regions |
| **Billing** | Direct | AWS billing | GCP billing |
| **Enterprise Features** | Basic | Advanced | Advanced |
| **Compliance** | Standard | AWS compliance | GCP compliance |

## Best Practices for Enterprise Deployment

1. **Use OIDC/Workload Identity** for credential management
2. **Implement quota monitoring** to avoid service interruptions
3. **Set up proper IAM roles** with least privilege access
4. **Configure region preferences** based on data residency requirements
5. **Enable logging and monitoring** for audit trails
6. **Use environment-specific configurations** for dev/staging/prod
7. **Implement cost controls** with budget alerts
8. **Test failover scenarios** between regions
9. **Document credential rotation procedures**
10. **Review security policies** regularly

## CI/CD Integration

Both Bedrock and Vertex AI support automated workflows:

**GitHub Actions with Bedrock:**
```yaml
- name: Configure AWS Credentials
  uses: aws-actions/configure-aws-credentials@v1
  with:
    role-to-assume: arn:aws:iam::ACCOUNT:role/ROLE
    aws-region: us-east-1

- name: Run Claude Code
  run: |
    export CLAUDE_CODE_USE_BEDROCK=1
    claude -p "task" --output-format json
```

**GitLab CI with Vertex AI:**
```yaml
script:
  - gcloud auth activate-service-account --key-file=$GCP_KEY_FILE
  - export CLAUDE_CODE_USE_VERTEX=1
  - export ANTHROPIC_VERTEX_PROJECT_ID=$PROJECT_ID
  - claude -p "task"
```
