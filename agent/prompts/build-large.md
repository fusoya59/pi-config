---
name: build-large
description: Implement a large-complexity feature from high-level plan file. Planner refinement, full review, coder, review loops.
argument-hint: "<plan-file-path>"
---

# Implement from $1 — large scope.

## Flow

### 1. Read Plan

Read high-level plan at $1. Understand scope, approach, risks, and suggested affected components.

### 2. Branch Strategy

Ask user for base branch. Propose branch name from plan.
Ask for review cycles $N$ (default 2).
Wait. Checkout branch.

### 3. Deepen Context

`Agent("Explore", model: "opencode-go/deepseek-v4-flash", thinking: "high", prompt: "<plan scope: affected files/modules>", desc: "Explore large context")`
Focus on files listed in plan — understand their structure, patterns, imports, and interfaces.

### 4. Refine Impl Plan

`Agent("Planner", model: "opencode-go/deepseek-v4-pro", thinking: "high", prompt: "Convert high-level plan + Explore results into concrete implementation plan. Plan: <plan file $1>. Context: <explore results>. Include exact files to create/modify, step-by-step order, key data structures/API/component interfaces, edge cases, migration/backward compatibility concerns if relevant, and test approach. Do not write code.", desc: "Refine large plan")`

Planner outputs concrete implementation plan inline.

### 5. Plan Review

`Agent("Plan-Reviewer",
  model: "opencode-go/deepseek-v4-pro",
  thinking: "high",
  prompt: "FULL REVIEW. Staff-level audit.
    Plan: <impl plan>. Context: <explore results>.
    Logical flaws? Security? Missing edge cases? Downstream effects?
    Output: <!-- STATUS: APPROVED --> or <!-- STATUS: CHANGES REQUESTED --> with specific issues.")`

On `<!-- STATUS: APPROVED -->` → proceed.
On `<!-- STATUS: CHANGES REQUESTED -->` → main agent fixes impl plan inline, then re-review with Plan-Reviewer.
If still not approved after one revision, escalate to user.

### 6. Coder

`Agent("Coder", model: "opencode-go/deepseek-v4-flash", thinking: "xhigh", prompt: "<approved impl plan + explore context>", desc: "Implement large feature")`

### 7. Code Review ↔ Fix

`Agent("Code-Reviewer", model: "opencode-go/deepseek-v4-flash", thinking: "xhigh", prompt: "<plan + diff>", desc: "Review large impl")`
On `<!-- STATUS: LGTM -->` → proceed.
On `<!-- STATUS: REVISION NEEDED -->` → send feedback to Coder.
Repeat review loop up to $N$ times. If still not LGTM after $N$ loops, escalate to user.

### 8. Validate

Run tests, linter, type-check. Fix if red.

### 9. User Review Before Commit

Show diff + summary. **Wait for user review.**

- Tweaks requested → apply, re-show diff.
- Approved → commit.

### 10. Commit

`git commit` with conventional commit message. Never push.
