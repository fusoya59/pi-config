---
name: build-small
description: Implement a small-scope feature from plan file. Like quick but with branch alignment.
argument-hint: "<plan-file-path>"
---

# Implement from $1 — small scope.

## Mode

Direct implementation. One Coder pass, one review pass.
Use any prior exploration context from this session.

## Flow

### 1. Read Plan

Read the plan file at $1. Understand scope, files affected, approach.

### 2. Branch Strategy

Ask user for base branch. Propose branch name from plan.
Wait for confirmation. Checkout branch.

### 3. Implement

Implement per plan. No scope creep. Update any docs that drifted out of date.

### 4. Review

`Agent("Code-Reviewer", model: "opencode-go/deepseek-v4-flash", thinking: "xhigh", prompt: "<context + diff>", desc: "Review small impl")`
On `<!-- STATUS: LGTM -->` → proceed.
On `<!-- STATUS: REVISION NEEDED -->` → apply changes, skip re-review.

### 5. Validate

Run relevant tests, linter, type-check. Fix any failures.

### 6. Commit

Show diff + summary. User approves → `git commit` with conventional commit message. Never push.
