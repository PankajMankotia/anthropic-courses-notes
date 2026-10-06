# Claude Code Best Practices: CLAUDE.md, Skills, Permission Modes & Hooks

## Overview

As Claude Code usage grows, success depends on using the right instruction surface for the right job:

| Tool | Purpose |
|--------|---------|
| **CLAUDE.md** | Conventions, guidance, and project rules |
| **Skills** | Reusable procedures and workflows |
| **Hooks** | Enforced behavior and guardrails |
| **Permission Modes** | Control Claude's autonomy |

A common mistake is trying to solve everything with a large `CLAUDE.md` file. Instead, distribute responsibilities appropriately and keep instruction systems lean and maintainable.

---

# 1. Managing CLAUDE.md Effectively

## The Core Principle

`CLAUDE.md` is **guidance, not enforcement**.

Every instruction competes for Claude's attention.

As the file grows:

- Instructions compete with each other.
- Compliance becomes less reliable.
- Important rules become diluted.

### Goal

Keep `CLAUDE.md`:

- Focused
- Lean
- High-signal
- Easy to follow

> The shorter and clearer the file, the more reliably Claude follows it.

---

## First Question: Does This Belong in CLAUDE.md?

Before adding a rule, determine whether it is:

### A. Guidance

Examples:

- Coding conventions
- Folder structure patterns
- Team preferences
- Naming standards

These belong in:

```text
CLAUDE.md
```

### B. Enforcement

Examples:

- Never push to main
- Never deploy to production directly
- Never expose secrets

These belong in:

```text
Hooks
```

Reason:

- CLAUDE.md asks Claude to comply.
- Hooks can prevent unwanted actions.

Example:

❌ Weak

```text
Never push to main.
```

✅ Enforced via PreToolUse hook

```text
Block git push origin main
```

---

# 2. The Four CLAUDE.md Locations

Claude loads all instruction layers together.

## 1. Managed Policy

Organization-wide instructions.

Characteristics:

- Managed centrally
- Cannot be excluded
- Always applied

Best for:

- Security policies
- Compliance rules
- Company standards

---

## 2. User

Personal preferences available across all repositories.

Best for:

- Preferred coding styles
- Personal workflows
- Global development preferences

---

## 3. Project

Stored in source control and shared by the entire team.

Best for:

- Team conventions
- Repository standards
- Shared workflows

---

## 4. Local

Git-ignored and specific to one repository.

Best for:

- Personal branch work
- Temporary project context
- Repository-specific notes

Example:

```text
Architectural decisions for a refactor branch
```

should go into:

```text
local CLAUDE.md
```

not the shared project file.

---

# 3. Organizing Large CLAUDE.md Files

## Use Imports

Instead of one large file:

```text
@.claude/conventions/code-style.md
@.claude/conventions/testing.md
@.claude/conventions/workflow.md
```

Benefits:

- Better organization
- Easier maintenance
- Logical separation of concerns

---

## Important Limitation

Imports do **not reduce context size**.

At startup:

```text
Imported files are expanded inline.
```

Claude still reads the entire content.

### Imports help:

✅ Organization

### Imports do not help:

❌ Reducing context load

---

# 4. Writing Effective Rules

## Make Rules Specific and Checkable

Bad:

```text
Follow API best practices.
```

Problem:

- Ambiguous
- Hard to verify

Better:

```text
Place new API routes in src/api/handlers,
one route per file.
```

Benefits:

- Clear
- Measurable
- Easily validated

---

## Always Provide the Alternative

Bad:

```text
Don't use default exports.
```

What should Claude use instead?

Better:

```text
Use named exports, not default exports.
```

The replacement is explicit.

---

## Treat Emphasis as a Budget

Words such as:

```text
IMPORTANT
YOU MUST
CRITICAL
```

increase instruction priority.

However:

If every rule is emphasized:

```text
Nothing stands out.
```

### Best Practice

Reserve emphasis for:

- 2–3 highest-risk rules
- Rules with severe consequences if broken

---

# 5. Continuously Refine CLAUDE.md

Treat the file like production code.

When Claude makes a mistake:

Don't merely fix the output.

Instead:

1. Identify the missing instruction.
2. Improve the rule.
3. Add it back to CLAUDE.md.

Think of failures as:

```text
Bugs in the instruction system.
```

---

# 6. Skills: Automating Repeatable Procedures

## What Is a Skill?

A skill is:

```text
A reusable procedure packaged into a folder.
```

At minimum:

```text
skill.md
```

contains:

- Name
- Description
- Trigger conditions
- Procedure

---

## Best First Skill: Verification

The most valuable first skill is usually:

```text
Verification
```

Why?

Manual verification depends on remembering to check.

A verification skill makes checking automatic.

---

## Example Verification Flow

After a refactor completes:

1. Run tests
2. Read diff
3. Verify tests were not weakened
4. Report pass/fail
5. Provide evidence

Outcome:

```text
Verification happens consistently.
```

without needing reminders.

---

## Rule of Thumb

If you have typed the same procedure twice:

```text
Make it a skill.
```

Examples:

- Release checklists
- Migration procedures
- PR validation
- Code reviews
- Refactoring verification

---

# 7. Skill Structure Beyond skill.md

A skill folder may contain:

```text
skill.md
reference.md
scripts/
```

---

## Reference Documents

Example:

```text
reference.md
```

Store:

- Detailed explanations
- Standards
- Supporting documentation

Benefit:

Claude only reads it when needed.

---

## Scripts

Example:

```text
check.sh
```

Uses:

- Run tests
- Run linters
- Execute validation workflows

Advantage:

Scripts execute directly rather than adding their content into context.

---

## Skill Design Rule

Keep:

```text
skill.md
```

short.

Move:

- Long explanations
- Reference material
- Tooling

into separate files.

---

# 8. Choosing the Correct Instruction Surface

## Use CLAUDE.md For

Always-on conventions.

Examples:

- Naming rules
- Directory structure
- Style standards

---

## Use Skills For

Task-specific procedures.

Examples:

- Verification workflows
- Release checklists
- PR preparation
- Migration tasks

---

## Use Hooks For

Rules that cannot be skipped.

Examples:

- Prevent pushes to main
- Secret protection
- Deployment restrictions

---

# 9. Permission Modes

Permission modes define how much autonomy Claude receives.

---

## 1. Manual

Allowed:

- Read operations

Everything else:

```text
Requires approval
```

Use for:

- High-control workflows

---

## 2. Accept Edits

Allowed:

- Reads
- File edits
- Common filesystem operations

Use for:

- Normal coding sessions

---

## 3. Plan

Allowed:

- Research
- Analysis

Not allowed:

- File modifications

Use for:

- Planning changes

---

## 4. Auto

Most autonomous mode.

Behavior:

- Claude acts independently.
- A classifier reviews each action.

Use for:

- Long-running implementation work

---

## 5. Don't Ask

Behavior:

- Pre-approved tools only
- Everything else denied automatically

Use for:

- CI pipelines
- Scheduled jobs
- Overnight automation

---

## 6. Bypass Permissions

Behavior:

- Removes permission checks

Use only inside:

- Containers
- Sandboxes
- Virtual machines

Never use on sensitive environments.

---

# 10. Understanding Auto Mode

## What Auto Mode Protects Against

Examples:

- Production deployments
- Force pushes
- Dangerous shell pipelines
- External data exfiltration
- Destructive actions

---

## What It Allows

Examples:

- Local edits
- Reading files
- Dependency installation
- Normal development workflows

---

## What It Cannot Verify

Auto mode checks:

```text
Intent
```

It does not verify:

```text
Correctness
```

Example:

- Broken authentication code may still pass permission review.

---

## Best Practice

Pair Auto Mode with:

```text
Stop Hooks
```

Together:

| Mechanism | Responsibility |
|------------|---------------|
| Auto Mode | Intent safety |
| Stop Hook | Code correctness |

---

# 11. Hooks: Enforced Guardrails

## Why Hooks Matter

CLAUDE.md says:

```text
Please do this.
```

Hooks say:

```text
You cannot avoid doing this.
```

Hooks are deterministic code.

---

# 12. Important Hook Events

## PreToolUse

Runs:

```text
Before tool execution
```

Best for:

- Blocking actions
- Approval enforcement
- Command rewriting

---

## PostToolUse

Runs:

```text
After tool execution
```

Best for:

- Formatting
- Linting
- Logging

---

## Stop

Runs:

```text
Before Claude ends a turn
```

Can force additional work if requirements are unmet.

---

## SessionStart

Runs:

```text
At session start
```

Useful for:

- Environment setup
- Context restoration

---

## PreCompact / PostCompact

Run around conversation compaction.

Useful for:

- State preservation
- Compression workflows

---

## InstructionsLoaded

Runs when instruction files are loaded.

Useful for:

- Auditing active rules

---

# 13. PreToolUse Decisions

PreToolUse returns structured JSON.

Supported decisions:

```json
allow
deny
ask
```

### Meaning

| Decision | Behavior |
|-----------|-----------|
| allow | Execute tool |
| deny | Block tool |
| ask | Request user approval |

---

## Rewriting Input

Instead of blocking:

```json
updatedInput
```

can modify a command.

Example:

### Original

```bash
curl API_KEY=sk_live_123...
```

### Rewritten

```bash
curl API_KEY=REDACTED
```

Benefits:

- Work continues
- Secrets remain protected

---

# 14. Hook Exit Codes

## Exit Code 0

Success.

- JSON processed
- Text may be injected into context on certain events

---

## Exit Code 2

Blocking error.

Typically:

```text
Stops execution
```

---

## Exit Code 1

Common misconception:

```text
Does NOT block execution.
```

If you need enforcement:

```text
Use exit code 2.
```

---

# 15. Preserving Context Across Compaction

Problem:

Compaction removes conversation history.

Solution:

Use:

```text
SessionStart
```

with a compaction matcher.

Process:

1. Generate state summary
2. Re-inject summary into context
3. Resume work with continuity

Example data:

- Active files
- Current task
- Architectural decisions

---

# Key Takeaways

## CLAUDE.md

- Keep it lean.
- Make rules specific.
- Use imports for organization.
- Continuously refine instructions.

## Skills

- Automate repeated workflows.
- Build a verification skill first.
- Package procedures, scripts, and references together.

## Permission Modes

- Match autonomy to the task.
- Use Auto for hands-off work.
- Use Don't Ask for unattended automation.
- Reserve Bypass for isolated environments.

## Hooks

- Enforce critical rules.
- Use PreToolUse for guardrails.
- Use Stop for quality gates.
- Preserve context via SessionStart.

---

# Decision Matrix

| Requirement | Best Tool |
|------------|-----------|
| Naming conventions | CLAUDE.md |
| Code style rules | CLAUDE.md |
| Verification workflow | Skill |
| Release checklist | Skill |
| Block push to main | Hook |
| Secret redaction | Hook |
| Preserve context after compact | Hook |
| Team-wide standards | Project CLAUDE.md |
| Personal preferences | User CLAUDE.md |
| Repository-specific notes | Local CLAUDE.md |

**Golden Rule:** Keep `CLAUDE.md` lean, move repeatable procedures into skills, and enforce critical requirements with hooks.
