---
name: pnb
description: Plan, estimate, then auto-chain matching build prompt.
argument-hint: "<feature idea>"
---

# PNB: plan → build

Brief: $ARGUMENTS

1. Read/follow `prompts/plan.md` fully for brief.
2. After plan file exists, build by estimate:
   - S → `prompts/build-small.md`
   - M → `prompts/build-medium.md`
   - L → `prompts/build-large.md`
3. Pass written plan path as builder plan arg.
4. If estimate missing/ambiguous, ask user.

Keep builder gates: branch confirm, review/validation, user approval before commit, never push. No scope creep.
