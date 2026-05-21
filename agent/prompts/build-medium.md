---
name: build-medium
description: Implement a medium-scope feature from high-level plan file. Explore, Planner refinement, light review, coder, user gates commit.
argument-hint: "<plan-file-path>"
---

# Implement from $1 — medium scope.

## Flow

### 1. Read Plan

Read high-level plan at $1. Understand scope, approach, risks, and suggested affected components.

### 2. Branch Strategy

Ask user for base branch. Propose branch name from plan.
Wait. Checkout branch.

### 3. Deepen Context

`Agent("Explore", model: "opencode-go/deepseek-v4-flash", thinking: "high", prompt: "<plan scope: affected files/modules>", desc: "Explore medium context")`
Focus on files listed in plan — understand their structure, patterns, imports, and interfaces.

### 4. Refine Impl Plan

`Agent("Planner", model: "opencode-go/deepseek-v4-flash", thinking: "high", prompt: "Convert high-level plan + Explore results into concrete implementation plan. Plan: <plan file $1>. Context: <explore results>. Include exact files to create/modify, step-by-step order, key data structures/API/component interfaces, edge cases, and test approach. Do not write code.", desc: "Refine medium plan")`

Planner outputs concrete implementation plan inline.

### 5. Light Plan Review

`Agent("Plan-Reviewer",
  model: "opencode-go/deepseek-v4-flash",
  thinking: "xhigh",
  prompt: "LIGHT REVIEW. Quick sanity check. One pass.
    Plan: <impl plan>. Context: <explore results>.
    Gaps? Missing files? Contradictions with existing code?
    Output: <!-- STATUS: APPROVED --> or <!-- STATUS: CHANGES REQUESTED --> with 1-2 concise issues.")`

On CHANGES REQUESTED → main agent fixes plan inline, no re-review.
On APPROVED → proceed.

### 6. Coder

`Agent("Coder", model: "opencode-go/deepseek-v4-flash", thinking: "high", prompt: "<impl plan + explore context>", desc: "Implement medium feature")`

### 7. Code Review

`Agent("Code-Reviewer", model: "opencode-go/deepseek-v4-flash", thinking: "xhigh", prompt: "<plan + diff>", desc: "Review medium impl")`
On `<!-- STATUS: LGTM -->` → proceed.
On `<!-- STATUS: REVISION NEEDED -->` → send feedback to Coder, one revision loop max.

### 8. Validate

Run tests, linter, type-check. Fix if red.

### 9. User Review Before Commit

Show diff + summary. **Wait for user review.**

- Tweaks requested → apply, re-show diff.
- Approved → commit.

### 10. Commit

`git commit` with conventional commit message. Never push.
