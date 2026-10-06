# Claude Code Automation & GitHub Integration

## Overview

Once you trust Claude to perform a task correctly, the next step is **automation**.

There are two primary automation paths:

1. **Automation of recurring work**
   - Routines
   - Headless mode
   - Agent SDK

2. **Automation inside GitHub workflows**
   - Managed Code Review
   - Claude GitHub Action

Think of it as a spectrum:

```text
Routines
    ↓
Headless Mode (-p)
    ↓
Headless Mode + Sessions
    ↓
Deterministic CI (--bare)
    ↓
Agent SDK
```

Move down the spectrum only when you need more control.

---

# 1. Automation Spectrum

| Tool | Best For |
|--------|----------|
| Routines | Recurring jobs with minimal setup |
| Headless Mode | Script and pipeline automation |
| Sessions | Multi-step workflows |
| Bare Mode | Deterministic CI execution |
| Agent SDK | Embedding Claude into applications |

---

# 2. Routines

## What Is a Routine?

A routine is:

```text
A saved Claude workflow that runs in the cloud.
```

A routine bundles:

- Prompt
- Repository
- Connectors
- Trigger

and executes automatically.

---

## Why Use Routines?

You do not need:

- Scripts
- Servers
- Cron jobs
- Workflow files

Anthropic manages the infrastructure.

---

## Typical Use Cases

### Daily Dependency Audit

```text
Run every morning
```

### PR Triage

```text
Triggered by pull requests
```

### Issue Prioritization

```text
Review Sentry tickets daily
```

### Status Reporting

```text
Generate recurring summaries
```

---

# 3. Routine Triggers

A routine can run from:

## Cron Schedule

Example:

```text
9:00 AM every day
```

---

## HTTP POST

Example:

```text
Your application triggers the routine
```

---

## GitHub Events

Examples:

- Pull request opened
- Repository activity
- Workflow events

---

# 4. Creating Routines

## Option 1: Web UI

Create from:

```text
claude.ai/code/routines
```

Provide:

- Name
- Instructions
- Repository
- Trigger

---

## Option 2: Claude Code

Use:

```bash
/schedule daily dependency audit at 9am
```

Claude creates the routine directly.

---

# 5. Routine Limitations

Before depending on routines heavily:

### 1. Research Preview

Behavior may change.

---

### 2. Schedule Frequency

Maximum recurring frequency:

```text
Hourly
```

Not suitable for minute-level polling.

---

### 3. Branch Safety

Each run:

```text
Starts from a fresh clone
```

By default it can only push to:

```text
claude/*
```

branches.

This prevents accidental modifications to main.

---

# 6. Headless Mode

## What Is Headless Mode?

Headless mode runs Claude as:

```bash
claude -p
```

without an interactive UI.

---

## Characteristics

- Reads STDIN
- Writes STDOUT
- Behaves like a shell utility
- Works inside scripts and pipelines

---

## Example

```bash
claude -p "summarize the changes in this diff"
```

---

## Best Use Cases

- Bash scripts
- CI jobs
- Scheduled automation
- Data extraction

---

# 7. What Headless Mode Does NOT Load

Headless mode intentionally skips auto-discovery of:

- Hooks
- Skills
- Plugins
- MCP servers
- CLAUDE.md

Result:

```text
Faster startup
```

and

```text
More predictable execution
```

---

# 8. Structured JSON Output

Instead of natural-language responses, Claude can return structured data.

---

## Using JSON Schema

Example:

```bash
claude -p "Extract function names" \
  --output-format json \
  --json-schema '{ ... }'
```

---

## Benefits

Output becomes:

```text
Machine-readable
```

and can be piped into:

- jq
- Databases
- APIs
- Automation scripts

---

## Typical Use Cases

- Code analysis
- Metadata extraction
- Report generation
- Pipeline integration

---

# 9. Multi-Step Automation with Sessions

Some workflows need multiple executions.

Instead of one massive prompt:

### Step 1

Create plan.

### Step 2

Continue from existing context.

---

## Resume a Session

```bash
claude --resume SESSION_ID
```

---

## Typical Pattern

### First Run

```text
Analyze
Plan
Store session ID
```

### Second Run

```text
Resume
Execute
```

---

## Best For

- Long workflows
- Staged deployments
- Planning then implementation

---

# 10. Deterministic CI with Bare Mode

## Problem

CI pipelines often require:

```text
Identical results every run
```

---

## Solution

```bash
--bare
```

mode.

---

## Benefits

- Predictable output
- Repeatability
- Consistency

---

## Best Use Cases

- CI pipelines
- Compliance checks
- Automated validation

---

# 11. Agent SDK

## What Is It?

The Agent SDK embeds Claude Code inside:

- TypeScript applications
- Python applications

---

## What It Provides

A programmable API for:

```text
Claude Code's capabilities
```

inside your own product.

---

## Configuration Options

Common controls:

### Allowed Tools

```text
What Claude can access
```

---

### System Prompt

```text
Behavior guidance
```

---

### Permission Mode

```text
Operational restrictions
```

---

## Best Use Cases

- Internal tooling
- SaaS products
- Development platforms
- AI-enabled applications

---

# 12. Choosing the Right Automation Approach

## Use Routines When

✅ Work repeats regularly

✅ No infrastructure is needed

✅ Cloud execution is acceptable

---

## Use Headless Mode When

✅ You need shell scripting

✅ Work belongs inside a pipeline

✅ Outputs feed other tools

---

## Use Bare Mode When

✅ Deterministic CI behavior is required

---

## Use Agent SDK When

✅ Claude must be embedded into your application

---

# 13. Pull Request Automation

Pull requests are the highest-value automation target because they contain:

- Reviews
- Validation
- Change management
- Engineering workflow

Claude offers two approaches:

1. Managed Code Review
2. GitHub Action

---

# 14. Managed Code Review

## What Is It?

An Anthropic-hosted PR review service.

Provided through:

```text
Claude GitHub App
```

---

## Key Characteristics

- No hosting required
- No custom workflows
- Reviews pull requests automatically
- Posts inline findings

---

## Setup

An organization administrator:

1. Enables Code Review
2. Installs the GitHub app
3. Selects repositories
4. Chooses trigger behavior

---

# 15. Code Review Triggers

Review can run:

### On PR Creation

```text
Review once when opened
```

---

### On Every Push

```text
Re-review updated changes
```

---

### On Request

```text
@claude review
```

---

# 16. How Managed Review Works

Review agents analyze:

```text
Changed code
+
Full repository context
```

not just the diff.

---

## Output

Claude provides:

- Inline comments
- Severity labels
- Findings summary

---

## Benefits

### Deduplication

Avoids repetitive comments.

### Prioritization

Highlights meaningful issues.

### Context Awareness

Uses repository-wide knowledge.

---

# 17. What Managed Review Does NOT Do

## It Does Not

### Approve PRs

Humans remain responsible.

---

### Block PRs

Findings are advisory.

---

### Auto-Fix Findings

Review only.

No managed code modification.

---

# 18. Applying Fixes Locally

For fixes:

```bash
/code-review --fix
```

Workflow:

```text
PR Finding
    ↓
Local Review
    ↓
Apply Fix
```

---

# 19. GitHub Action

## When to Use It

Use the action when Claude must:

- Modify code
- Execute workflows
- Generate reports
- Automate CI tasks

---

## Typical Tasks

### Implement Changes

```text
@claude implement
```

---

### Scheduled Reports

```text
Daily summaries
```

---

### Workflow Automation

```text
Issue handling
Documentation generation
Code modifications
```

---

# 20. GitHub Action Setup

Begin with:

```bash
/install-github-app
```

Requirements:

- Repository admin access
- Anthropic API key

---

## Required Components

### GitHub App

Installed into repository.

### Repository Secret

```text
ANTHROPIC_API_KEY
```

---

# 21. Core Action Inputs

## API Key

```text
anthropic_api_key
```

---

## GitHub Token

```text
github_token
```

---

## Trigger Phrase

Example:

```text
@claude
```

---

## Prompt

Instructions for Claude.

---

## Claude Arguments

CLI flags controlling behavior.

---

# 22. Comment-Driven Workflows

Example:

```text
@claude implement this feature
```

The action:

1. Detects comment
2. Executes Claude
3. Applies changes
4. Pushes commits
5. Reports results

---

# 23. Scheduled Workflows

GitHub cron can trigger Claude automatically.

Examples:

- Daily reports
- Code audits
- Dependency reviews
- Health checks

---

## Manual Execution

Use:

```text
workflow_dispatch
```

to trigger manually.

---

# 24. Tuning with claude_args

Most action customization happens through:

```text
claude_args
```

---

## Control Maximum Turns

Example:

```bash
--max-turns 5
```

Prevents endless execution.

---

## Permission Modes

Choose appropriate autonomy.

For unattended runs:

```text
No approval prompts
```

---

## Allowed Tools

Grant only required permissions.

Examples:

### Reporting Job

```text
Read-only access
```

### Implementation Job

```text
Code modification permissions
```

---

# Best Practices

## Automation Hierarchy

Start with:

```text
Routine
```

Move down only when needed.

```text
Routine
    ↓
Headless Mode
    ↓
Sessions
    ↓
Bare Mode
    ↓
Agent SDK
```

---

## PR Automation Hierarchy

For Review:

```text
Managed Code Review
```

For Action:

```text
GitHub Action
```

---

# Quick Decision Matrix

| Requirement | Recommended Tool |
|------------|------------------|
| Daily recurring task | Routine |
| Scheduled cloud automation | Routine |
| Pipeline scripting | Headless Mode |
| JSON extraction | Headless Mode |
| Multi-step workflow | Sessions |
| Deterministic CI | Bare Mode |
| Embed Claude in product | Agent SDK |
| PR review comments | Managed Code Review |
| Automated code changes in GitHub | GitHub Action |
| Scheduled GitHub reports | GitHub Action |
| Comment-driven workflows | GitHub Action |

---

# Key Takeaways

### Routines

- Simplest automation option.
- Managed infrastructure.
- Best for recurring tasks.

### Headless Mode

- Scriptable and pipeline-friendly.
- Supports structured JSON output.

### Sessions

- Enable multi-stage automation.

### Bare Mode

- Provides deterministic CI execution.

### Agent SDK

- Embeds Claude into applications.

### Managed Code Review

- Best for automated PR feedback.
- Review only, no fixing.

### GitHub Action

- Best for implementing work in CI/CD.
- Supports comments
