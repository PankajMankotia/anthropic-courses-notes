# Claude Code Verification & Plugins

## Overview

As Claude becomes more autonomous, verification becomes increasingly important.

**Golden Rule:**

> The less you supervised a run, the more rigorously you should verify it afterward.

Similarly, once you build a Claude setup that works well, plugins allow you to package and share it across your team instead of manually copying configuration files.

---

# Part 1: Verifying Unsupervised Claude Runs

## Core Principle

Verification should be proportional to autonomy.

### Supervised Run

You watched:

- Prompts
- Tool calls
- Outputs

Verification:

```text
Light review
```

is usually sufficient.

---

### Unattended Run

Nobody watched:

- Actions taken
- Files modified
- Decisions made

Verification:

```text
Full validation required
```

---

## Verification Pyramid

```text
Most Trust Required
    ↓
Unattended / CI Runs
    ↓
Headless Automation
    ↓
Long Autonomous Sessions
    ↓
Interactive Sessions
    ↓
Least Verification Required
```

---

# 1. Keep Unattended Runs in Auto Mode

## Recommended Mode

Use:

```text
Auto Mode
```

instead of:

```text
Bypass Permissions
```

for unattended work.

---

## Why Auto Mode?

Auto Mode provides:

```text
Intent Safety
```

via a classifier that reviews actions before they run.

Examples of dangerous actions it may block:

- Production deployments
- Destructive commands
- Dangerous repository operations
- Sensitive external communication

---

## Important Limitation

The classifier checks:

✅ Intent

It does **not** check:

❌ Correctness

Example:

```text
Broken code may still pass classifier review.
```

Verification is still required.

---

# 2. Start with the Diff, Not the Summary

## Common Mistake

Reading Claude's summary first.

Example:

```text
"Refactored authentication module."
```

Sounds good.

But:

```text
What files actually changed?
```

may tell a different story.

---

## Correct Process

### Step 1

Run:

```bash
/code-review
```

and examine findings.

---

### Step 2

Open:

```bash
git diff
```

---

### Step 3

Review all changes manually.

Focus on:

- Intended files
- Unexpected files
- Unplanned modifications

---

## Why This Matters

A summary tells you:

```text
What Claude claims happened.
```

A diff shows:

```text
What actually happened.
```

Trust the diff.

---

# 3. Treat Tests as a Gate, Not a Promise

## Bad Approach

Trusting statements such as:

```text
All tests passed.
```

---

## Better Approach

Enforce testing with hooks.

Claude should prove:

```text
Tests passed.
```

not merely claim it.

---

# 4. Verification Hooks

## Stop Hook

Runs before Claude finishes.

Purpose:

- Execute test suite
- Block completion on failure

---

### Example Flow

```text
Claude finishes
      ↓
Stop Hook runs tests
      ↓
Tests fail
      ↓
Exit 2
      ↓
Claude continues fixing
```

---

## PostToolUse Hook

Runs after edits.

Typical tasks:

- Formatting
- Linting
- Type checking

---

### Example Flow

```text
File Edited
      ↓
PostToolUse
      ↓
Lint
Type Check
      ↓
Failure Returned
      ↓
Claude Fixes Issue
```

---

# 5. Why Exit Code 2 Matters

A hook that exits with:

```text
exit 2
```

returns the failure to Claude.

Claude sees:

```text
Test failed
Lint failed
Type check failed
```

and attempts to repair the issue automatically.

---

## Benefit

The quality gate runs:

✅ Every time

instead of:

❌ Only when you remember to ask

---

# 6. Get a Cold Second Opinion

## Problem

The author agent just spent a long time solving the problem.

It may become attached to:

- Design assumptions
- Workarounds
- Incorrect reasoning

---

## Solution

Use:

```text
Fresh Session
```

or

```text
Independent Sub-Agent
```

to review the work.

---

## Why It Works

The reviewer has:

```text
No emotional attachment
```

to the implementation.

It evaluates:

- Diff
- Tests
- Logic

with fresh context.

---

# 7. Verify Headless Runs

Headless runs should be validated using:

## Exit Codes

```text
Success/Failure
```

---

## Structured JSON Output

```json
{
  "status": "success"
}
```

---

## Validation Rules

Never rely solely on:

```text
Human-readable summaries
```

Prefer:

- Exit code
- Structured output
- Test results

