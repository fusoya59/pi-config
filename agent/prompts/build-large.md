---
name: build-large
description: Implement a large-complexity feature from plan file. Full feature flow sans alignment.
argument-hint: "<plan-file-path>"
---

# Implement from $1 — large scope.

## Flow

### 1. Read Plan

Read $1. Full understanding of scope and approach.

### 2. Branch Strategy

Ask user for base branch. Propose branch name from plan.
Ask for review cycles $N$ (default 2).
Wait. Checkout branch.

### 3. Deepen Context

`Agent("Explore", model: "opencode-go/deepseek-v4-flash", thinking: "high", prompt: "<plan scope: affected files/modules>", desc: "Explore large context")`
Focus on files listed in plan — understand their structure, patterns, imports, and interfaces.

### 4. Plan Review

`Agent("Plan-Reviewer",
  model: "opencode-go/deepseek-v4-pro",
  thinking: "high",
  prompt: "FULL REVIEW. Staff-level audit.
    Plan: <plan file $1>. Context: <explore results>.
    Logical flaws? Security? Missing edge cases? Downstream effects?
    Output: <!-- STATUS: APPROVED --> or <!-- STATUS: CHANGES REQUESTED --> with specific issues.")`

On `<!-- STATUS: APPROVED -->` → proceed.
On `<!-- STATUS: CHANGES REQUESTED -->` → main agent fixes plan inline, then re-review with Plan-Reviewer.
If still not approved after one revision, escalate to user.

### 5. Coder

`Agent("Coder", model: "opencode-go/deepseek-v4-flash", thinking: "xhigh", prompt: "<approved plan + explore context>", desc: "Implement large feature")`

### 6. Code Review ↔ Fix

`Agent("Code-Reviewer", model: "opencode-go/deepseek-v4-flash", thinking: "xhigh", prompt: "<plan + diff>", desc: "Review large impl")`
On `<!-- STATUS: LGTM -->` → proceed.
On `<!-- STATUS: REVISION NEEDED -->` → send feedback to Coder.
Repeat review loop up to $N$ times. If still not LGTM after $N$ loops, escalate to user.

### 7. Validate

Run tests, linter, type-check. Fix if red.

### 8. User Review Before Commit

Show diff + summary. **Wait for user review.**

- Tweaks requested → apply, re-show diff.
- Approved → commit.

### 9. Commit

`git commit` with conventional commit message. Never push.
