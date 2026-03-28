---
name: prd-to-linear-issues
description: Break a PRD into independently-grabbable Linear issues using tracer-bullet vertical slices. Use when user wants to convert a PRD to Linear issues, create Linear implementation tickets, or break down a PRD into Linear work items.
---

# PRD to Linear Issues

Break a PRD into independently-grabbable Linear issues using vertical slices (tracer bullets).

## Process

### 1. Verify Linear workspace and select team

Call `mcp__linear-server__get_authenticated_user` to confirm the connection. Then call `mcp__linear-server__list_teams`.

If there's one team, use it. If multiple, ask the user which to use.

### 2. Select or create a Linear project

Call `mcp__linear-server__list_projects` and present existing projects. Ask the user to pick one or provide a name to create a new one via `mcp__linear-server__save_project`.

### 3. Ensure labels exist

Call `mcp__linear-server__list_issue_labels` and check for:

- **Type labels**: `feature`, `bug`, `improvement`
- **Discipline labels**: `design`, `engineering`

Create any missing labels via `mcp__linear-server__create_issue_label`.

### 4. Locate the PRD

Ask the user for the PRD GitHub issue number (or URL). Fetch it with `gh issue view <number>`.

### 5. Explore the codebase (optional)

If you have not already explored the codebase, do so to understand the current state of the code.

### 6. Draft vertical slices

Break the PRD into **tracer bullet** issues. Each issue is a thin vertical slice that cuts through ALL integration layers end-to-end, NOT a horizontal slice of one layer.

Slices may be **HITL** (requires human interaction — architectural decision, design review) or **AFK** (can be implemented without human interaction). Prefer AFK where possible.

<vertical-slice-rules>
- Each slice delivers a narrow but COMPLETE path through every layer (schema, API, UI, tests)
- A completed slice is demoable or verifiable on its own
- Prefer many thin slices over few thick ones
</vertical-slice-rules>

### 7. Determine discipline issues per slice

For each slice, determine which **discipline issues** need to be created:

- **Design issue** — needed when the slice involves UI/UX work, visual design, user flows, wireframes, or design system changes.
- **Engineering issue** — needed when the slice involves code implementation, API work, database changes, or infrastructure.

Most slices will require **both** a design and an engineering issue. Some slices may only need one (e.g., a pure API refactor needs only engineering; a brand guideline update needs only design).

When both are needed, the **design issue blocks the engineering issue** within the same slice — design must be completed before engineering can begin implementation.

### 8. Quiz the user

Present the proposed breakdown as a numbered list. For each slice, show:

- **Title**: short descriptive name
- **Type**: HITL / AFK
- **Discipline issues**: which issues will be created (design, engineering, or both)
- **Type label**: `feature`, `bug`, or `improvement`
- **Estimates**: Fibonacci points (1, 2, 3, 5, 8, 13) per discipline issue
- **Blocked by**: which other slices (if any) must complete first (cross-slice dependencies)

Example format:

```
1. User onboarding flow
   Type: AFK | Label: feature
   - [Design] Onboarding screens and flow — 3 pts
   - [Engineering] Implement onboarding — 5 pts (blocked by Design)
   Blocked by: none

2. Payment integration
   Type: HITL | Label: feature
   - [Engineering] Stripe API integration — 8 pts
   Blocked by: Slice 1
```

Ask the user:

- Does the granularity feel right?
- Are the dependency relationships correct?
- Should any slices be merged or split further?
- Are the HITL/AFK assignments correct?
- Are the discipline splits correct? (should any gain/lose a design or engineering issue?)
- Do the estimates and labels look right?

Iterate until the user approves.

### 9. Create Linear issues

Create issues in dependency order: within each slice, create the design issue before the engineering issue so you can reference the design issue ID as a blocker.

For each discipline issue, call `mcp__linear-server__save_issue` with:

- **Title**: `[Design] Slice title` or `[Engineering] Slice title`
- **Description**: use the template below
- **Team**: the selected team
- **Project**: the selected project
- **Labels**: the discipline label (`design` or `engineering`) + the type label (`feature`/`bug`/`improvement`)
- **Estimate**: the approved Fibonacci points for this discipline issue

<issue-description-template>
## Parent PRD

[PRD Title](https://github.com/OWNER/REPO/issues/NUMBER)

## What to build

A concise description of what this discipline's contribution to the slice entails. For design issues, describe the deliverables (screens, flows, specs). For engineering issues, describe the end-to-end behavior to implement.

## Acceptance criteria

- [ ] Criterion 1
- [ ] Criterion 2
- [ ] Criterion 3

## Blocked by

- Blocked by ISSUE-ID (if any — includes both cross-slice and within-slice design→engineering dependencies)

Or "None — can start immediately" if no blockers.
</issue-description-template>

Do NOT close or modify the parent PRD GitHub issue.
