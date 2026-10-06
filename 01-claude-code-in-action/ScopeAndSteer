# Managing Long Claude Code Sessions

## Key Idea

Short tasks with Claude are straightforward, but long-running tasks, such as large refactors, multi-file changes, or new feature development, require better planning and guidance.

Success comes from two main habits:

1. **Scope the work before Claude starts.**
2. **Steer Claude while it runs.**

---

## 1. Scope Work First with Plan Mode

### What Plan Mode Does

- Runs in **read-only mode**.
- Analyzes the codebase and identifies the required changes.
- Produces an implementation plan before making edits.

### Best Practices

- Carefully review the plan before execution.
- Do not skim it.
- Identify missing or incorrect steps early.
- Ask Claude to adjust or expand specific parts of the plan.
- Iterate on the plan before coding begins.

### Benefits

- Reduces surprises during implementation.
- Prevents wasted effort and cleanup later.
- Makes the execution phase more predictable.
- Refining a plan is faster than correcting poorly scoped implementation work.

---

## 2. Steer Claude During Execution

### A. Compact

#### Purpose

- Summarizes the conversation.
- Replaces old messages with the summary.
- Frees up the context window so Claude can continue working.

#### Risk

Important details may be omitted from the summary, causing Claude to drift off course.

#### Best Practice

Do not run `/compact` without additional direction. Tell Claude what the summary should preserve.

```text
/compact Focus on the --version flag implementation
```

Anything added after the command helps shape the summary and retain the most relevant context.

#### Key Takeaway

Treat compaction as a steering mechanism, not merely a context-cleanup action.

---

### B. Rewind

#### Purpose

Rewind allows you to return to an earlier checkpoint when Claude starts moving in the wrong direction.

Every user prompt creates a checkpoint that can be restored.

#### How to Open the Rewind Menu

Double-tap **Escape** on an empty prompt.

#### Rewind Options

##### Restore Code and Conversation

Rolls back both the code changes and the conversation.

##### Restore Conversation

Rolls back only the chat while keeping file changes.

##### Restore Code

Rolls back only the file changes while preserving the conversation.

##### Summarize from Here

Summarizes everything after the selected checkpoint.

**Useful when:** You had a side conversation and want to free up context space.

##### Summarize up to Here

Summarizes everything before the selected checkpoint.

**Useful when:** You want to compress a long setup phase while keeping implementation details intact.

#### Key Takeaway

Rewind is often faster and cleaner than repeatedly prompting Claude to recover from an incorrect approach.

---

## 3. Increase Autonomy with Goal and Loop

### A. Goal

#### Purpose

`/goal` defines a completion condition. Claude continues working across turns until a fast evaluator confirms that the condition has been met.

Claude does not stop simply because it believes the task is complete.

#### Example

```text
/goal all tests in src/billing pass, and the type checker reports zero errors
```

#### Cancel a Goal

```text
/goal clear
```

#### Important Constraint

The evaluator only reads the transcript. Therefore, the completion condition must be verifiable from Claude's output, such as test results or type-checker output.

#### Best Use Case

Use `/goal` when you can define what **done** looks like more clearly than you can define every implementation step.

---

### B. Loop

#### Purpose

Loop runs a prompt repeatedly between turns, either at a fixed interval or at a self-paced interval.

#### Typical Uses

- Monitor a CI run.
- Check deployment status.
- Pull an external status.
- Take action when an external state changes.

#### Stop a Loop

Press **Escape**.

---

## 4. Run Parallel Work Safely with Worktrees

### Problem

Running multiple Claude sessions against the same codebase can cause file conflicts because multiple agents may modify the same files.

### Solution: Worktrees

Each Claude session receives its own independent file tree.

### Benefits

- Prevents sessions from overwriting each other's changes.
- Supports parallel agent workflows.
- Keeps each session isolated.
- Automatically removes a clean worktree when the session exits.

### `.worktreeinclude`

A `.worktreeinclude` file in the repository root lists git-ignored files that should be copied into each worktree.

#### Useful For

- Environment variable files
- Local configuration files
- Other uncommitted resources required in every worktree

---

## Recommended Workflow

### Before Starting

- [ ] Use **Plan Mode**.
- [ ] Review the complete implementation plan.
- [ ] Correct missing, unclear, or unnecessary steps.

### During Execution

- [ ] Use **Compact** with explicit instructions about what to preserve.
- [ ] Use **Rewind** when Claude drifts or takes an incorrect approach.

### For Autonomous Work

- [ ] Use **Goal** to define measurable completion criteria.
- [ ] Use **Loop** to monitor external processes or changing states.

### For Multiple Agents

- [ ] Use **Worktrees** to isolate sessions and prevent conflicts.
- [ ] Add required git-ignored files to `.worktreeinclude`.

---

## Quick Reference

| Tool or Feature | Purpose | Best Used When |
|---|---|---|
| **Plan Mode** | Researches the codebase and creates a plan before editing | Starting a large or complex task |
| **Compact** | Summarizes conversation history to free context | The session is long and context must be preserved selectively |
| **Rewind** | Restores code, conversation, or both to a checkpoint | Claude takes the wrong approach |
| **Goal** | Keeps Claude working until a verifiable condition is met | The desired outcome is easier to define than the steps |
| **Loop** | Repeats a prompt between turns | Monitoring CI, deployments, or another external state |
| **Worktrees** | Gives parallel sessions independent file trees | Multiple agents are working on the same repository |

---

## Summary

To manage long Claude Code sessions effectively:

1. **Plan before coding.**
2. **Guide conversation summaries with Compact.**
3. **Use Rewind to recover quickly from mistakes.**
4. **Use Goal when outcomes are clearer than implementation steps.**
5. **Use Loop for ongoing monitoring.**
6. **Use Worktrees for parallel development.**

These practices reduce supervision overhead and make long-running Claude sessions more reliable, controlled, and autonomous.
