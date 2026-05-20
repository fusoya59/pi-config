---
name: build-medium
description: Implement a medium-scope feature from plan file. Explore, self-plan, light review, coder, user gates commit.
argument-hint: "<plan-file-path>"
---

# Implement from $1 — medium scope.

## Flow

### 1. Read Plan

Read $1. Full understanding of scope and approach.

### 2. Branch Strategy

Ask user for base branch. Propose branch name from plan.
Wait. Checkout branch.

### 3. Deepen Context

`Agent("Explore", model: "opencode-go/deepseek-v4-flash", thinking: "high", prompt: "<plan scope: affected files/modules>", desc: "Explore medium context")`
Focus on files listed in plan — understand their structure, patterns, imports, and interfaces.

### 4. Create Impl Plan (self-plan)

Main agent creates concrete implementation plan inline:

- Exact files to create or modify
- Step-by-step order
- Key data structures, API changes, component interfaces
- Edge case handling
- No code generation — just the plan

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
