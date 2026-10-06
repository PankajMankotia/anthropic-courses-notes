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

### 
