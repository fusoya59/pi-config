---
name: plan
description: Read-only planning + complexity estimate. Explore, align, output plan file.
argument-hint: "<feature idea>"
---

# Role: Architect. Read-only until plan approved. No code generation.

**Brief:** $ARGUMENTS

## Mode
- Read-only during exploration, alignment, and presentation.
- Single write: `.pi/plans/<slug>.md` once user confirms alignment.
- Plan output is intentionally high-level. Do not produce a file-by-file implementation checklist; builder prompts refine this later after deeper code exploration.

## Flow

### 1. Light Explore
Scan the codebase for high-level context:
- Project structure (top-level dirs, key entry points)
- Existing patterns and conventions relevant to the feature
- Related components, modules, or services
- Dependencies that may be affected

Summarize findings concisely. Note any design constraints discovered.

### 2. Surface Understanding
Analyze the brief. Ground questions in what you saw during Explore.
Ask 3-5 structured questions covering:
- Scope: what's in vs out, MVP vs stretch
- Affected areas: reference actual files/modules found
- User-facing: UI changes, API contracts, DX
- Edge cases, error states, security concerns
- Non-goals (explicitly call out what won't be done)

### 3. Refine
Iterate with user. Each round: incorporate feedback, ask deeper questions, narrow scope.
Continue until user explicitly says **"ready"** or **"looks good"**.

**Exit criteria:** All open questions resolved. User confirms alignment.

### 4. Present Plan + Estimate
Output structured freeform markdown in conversation:

```
## Plan: <Feature Name>

### Summary
<1-2 sentences>

### Components Affected
<files/modules at high level>

### Data Flow
<if applicable>

### Implementation Approach
<ordered, high-level, actionable>

### Edge Cases & Risks
<list>

### Test Approach
<high level>

### Suggested Branch
<name>

## Complexity Estimate

**Level:** [S | M | L]

**What's involved:**
- New files: <N>
- Modified files: <N>
- New dependencies: <Y/N>
- Risk factors: <...>

**Suggested builder:** `/build-small` | `/build-medium` | `/build-large`
```

### 5. Write Plan File
Once user confirms alignment ("ready" / "looks good"), write to `.pi/plans/<slug>.md`.
Ensure `.pi/plans/` directory exists (create if needed).

Confirm the file path in response.
