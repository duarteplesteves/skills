---
name: prd-to-issues
description: Break a PRD into Linear issues using tracer-bullet vertical slices, with design/engineering sub-issues, priority, and Fibonacci estimates. Use when user wants to convert a PRD to Linear issues, create implementation tickets, or break down a PRD into work items.
---

# PRD to Linear Issues

Break a PRD into independently-grabbable Linear issues using vertical slices (tracer bullets). Issues are non-technical, include priority and Fibonacci estimates, and split into design/engineering sub-issues when both disciplines are needed.

## Prerequisites

Before starting, verify the **Linear MCP server** is configured and authenticated.
Call `mcp__linear-server__get_authenticated_user`. If it fails, tell the user:

> "The Linear MCP server is not configured. Please install and authenticate it before using this skill."

Then stop.

## Process

### 1. Select team

Call `mcp__linear-server__list_teams`. If one team, use it. If multiple, ask the user.

### 2. Select the Linear project

Call `mcp__linear-server__list_projects` and present existing projects. The issues should go under the most relevant domain project (e.g., "Appointments", "Payments", "Onboarding").

If no suitable project exists, create one via `mcp__linear-server__save_project`.

### 3. Ensure labels exist

Call `mcp__linear-server__list_issue_labels` and check for:

- **Role labels**: `admin`, `patient`, `shared`
- **Design status labels**: `needs-design`, `in-design`, `design-ready`, `no-design`

Create any missing labels via `mcp__linear-server__create_issue_label`.

### 4. Locate the PRD

Ask the user for the PRD source. This can be:
- A **Linear Document** (fetch via Linear MCP)
- A **local markdown file** (read from disk)
- A **PRD already in the conversation** from a previous `/write-a-prd` session

### 5. Explore the codebase

If you have not already explored the codebase, do so to understand the current state of the code and inform your breakdown.

### 6. Draft vertical slices

Break the PRD into **tracer bullet** issues. Each issue is a thin vertical slice that cuts through ALL integration layers end-to-end, NOT a horizontal slice of one layer.

<vertical-slice-rules>
- Each slice delivers a narrow but COMPLETE path through every layer (schema, API, UI, tests)
- A completed slice is demoable or verifiable on its own
- Prefer many thin slices over few thick ones
</vertical-slice-rules>

### 7. Determine design needs per slice

For each slice, determine if it needs design work:

- **Both design and engineering** — the slice involves UI/UX that needs to be designed before implementation. This creates a **parent issue** with two **sub-issues**: one for Design (assigned to the designer) and one for Engineering (assigned to the engineer). The design sub-issue blocks the engineering sub-issue.
- **Engineering only** — purely technical work with no design needed (e.g., API changes, database migrations, background jobs). This creates a **single issue** assigned to the engineer. Tagged with `no-design`.

### 8. Quiz the user

Present the proposed breakdown as a numbered list. For each slice, show:

- **Title**: short descriptive name
- **Role label**: `admin`, `patient`, or `shared`
- **Priority**: Urgent, High, Medium, or Low
- **Design needs**: design + engineering, or engineering only
- **Estimates**: Fibonacci points (1, 2, 3, 5, 8, 13, 21) per sub-issue
- **Blocked by**: which other slices (if any) must complete first

Example format:

```
1. Patient can book an appointment
   Role: patient | Priority: High
   - [Design] Booking flow screens — 3 pts (assigned to designer)
   - [Engineering] Implement booking flow — 5 pts (assigned to engineer, blocked by Design)
   Blocked by: none

2. Doctor views appointment in agenda
   Role: admin | Priority: High
   - [Engineering] Agenda calendar integration — 5 pts (assigned to engineer)
   Blocked by: Slice 1

3. Patient receives appointment confirmation email
   Role: shared | Priority: Medium
   - [Engineering] Confirmation email template + trigger — 2 pts (assigned to engineer)
   Blocked by: Slice 1
```

Ask the user:

- Does the granularity feel right?
- Are the dependency relationships correct?
- Should any slices be merged or split further?
- Are the design/engineering splits correct?
- Do the priorities and estimates look right?
- Are the role labels correct?

**Iterate until the user approves.**

### 9. Create Linear issues

Create issues in dependency order. For slices needing both design and engineering:

1. Create the **parent issue** (the slice title)
2. Create the **Design sub-issue** under the parent, assigned to the designer
3. Create the **Engineering sub-issue** under the parent, assigned to the engineer, blocked by the design sub-issue

For engineering-only slices, create a single issue assigned to the engineer.

#### Parent issue fields (when both disciplines needed)

Call `mcp__linear-server__save_issue` with:

- **Title**: Slice title
- **Description**: use the parent issue template below
- **Team**: the selected team
- **Project**: the selected project
- **Labels**: the role label (`admin`/`patient`/`shared`)
- **Priority**: the approved priority
- **Estimate**: total points (design + engineering)

#### Sub-issue fields

Call `mcp__linear-server__save_issue` with:

- **Title**: `[Design] Slice title` or `[Engineering] Slice title`
- **Description**: use the sub-issue template below
- **Team**: the selected team
- **Project**: the selected project
- **Parent**: the parent issue ID
- **Labels**: role label + design status label (`needs-design` for design issues, `no-design` for engineering-only)
- **Assignee**: designer for design issues, engineer for engineering issues
- **Priority**: same as parent
- **Estimate**: the approved Fibonacci points for this sub-issue

#### Single issue fields (engineering only)

Call `mcp__linear-server__save_issue` with:

- **Title**: Slice title
- **Description**: use the single issue template below
- **Team**: the selected team
- **Project**: the selected project
- **Labels**: role label + `no-design`
- **Assignee**: the engineer
- **Priority**: the approved priority
- **Estimate**: the approved Fibonacci points

## Issue Templates

<parent-issue-template>
## What

A concise, non-technical description of what this feature slice delivers from the user's perspective.

## Acceptance criteria

- [ ] Criterion 1
- [ ] Criterion 2
- [ ] Criterion 3

## Blocked by

- Blocked by ISSUE-ID (cross-slice dependencies, if any)

Or "None — can start immediately" if no blockers.
</parent-issue-template>

<sub-issue-template>
## What

For **design sub-issues**: describe the deliverables — screens, flows, components to design. Mention where to attach the Figma link when done.

For **engineering sub-issues**: describe the end-to-end behavior to implement. Non-technical — what the user should be able to do when this is done.

## Acceptance criteria

- [ ] Criterion 1
- [ ] Criterion 2
- [ ] Criterion 3

## Blocked by

- Blocked by ISSUE-ID (design sub-issue blocks engineering sub-issue within the same slice, plus any cross-slice dependencies)

Or "None — can start immediately" if no blockers.
</sub-issue-template>

<single-issue-template>
## What

A concise, non-technical description of what this delivers.

## Acceptance criteria

- [ ] Criterion 1
- [ ] Criterion 2
- [ ] Criterion 3

## Blocked by

- Blocked by ISSUE-ID (if any)

Or "None — can start immediately" if no blockers.
</single-issue-template>
