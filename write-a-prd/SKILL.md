---
name: write-a-prd
description: Create a lean PRD through a conversational interview, then publish it as a Linear Document under the relevant project. Use when user wants to write a PRD, create a product requirements document, plan a new feature, or capture a feature idea.
---

# Write a PRD

Transform a rough idea or brain dump into a well-structured PRD via a conversational interview, then publish it as a Linear Document in the relevant Linear project.

## Prerequisites

Before starting, verify the **Linear MCP server** is configured and authenticated.
Call `mcp__linear-server__get_authenticated_user`. If it fails, tell the user:

> "The Linear MCP server is not configured. Please install and authenticate it before using this skill."

Then stop.

## Process

### 1. Accept the brain dump

Ask the user for a rough description of their idea. Accept anything — a sentence, a paragraph, a messy brain dump. Do not ask for structure; that's your job.

### 2. Explore the codebase

Explore the repo to understand the current state of the codebase and verify any assumptions from the user's description. This context will inform your interview questions.

### 3. Interview the user

Interview the user to fill in the gaps. Ask questions **one at a time**, and adapt based on answers. For each question, provide your recommended answer based on what you've learned from the codebase and the conversation so far.

Cover these areas (skip what's already clear from the brain dump):

- **What** — clear description of the feature or change
- **Why** — the problem it solves or value it adds
- **Who** — which users are affected (patient, doctor/admin, or both)
- **How it looks** — key UI/UX notes, user flows, screen descriptions
- **Scope boundaries** — what is explicitly NOT included
- **Dependencies** — does it touch existing systems (payments, auth, calendar, etc.)

Keep the interview focused — aim for 5-10 minutes, not an exhaustive interrogation. Stop when you have enough to write a clear, actionable PRD.

### 4. Write the PRD

Generate the PRD using the template below. Present it to the user for review. Iterate until approved.

### 5. Publish to Linear

#### 5a. Select or create the Linear project

Call `mcp__linear-server__list_projects` and present existing projects. The PRD should go under the most relevant domain project (e.g., "Appointments", "Payments", "Onboarding").

If no suitable project exists, create one via `mcp__linear-server__save_project`.

#### 5b. Create the Linear Document

Call `mcp__linear-server__create_document` with:
- **Title**: the PRD title
- **Content**: the full PRD content (markdown)
- **Project**: the selected project

Confirm success and share the document link with the user.

### 6. Save locally to Obsidian

Save the PRD to:

```
/Users/duarteesteves/Documents/obsidian-notes/<project-name>/prds/<prd-name>.md
```

Where:
- `<project-name>` is the Linear project name (kebab-case)
- `<prd-name>` is derived from the PRD title (kebab-case)

Create directories if they don't exist.

## PRD Template

<prd-template>
## Problem Statement

The problem from the user's perspective. Why does this matter?

## Solution

A clear, non-technical description of what will be built. Written so a non-technical stakeholder can understand it.

## Who is affected

Which users this impacts: patients, doctors/admin, or both. Describe how each role interacts with the feature.

## User Stories

A numbered list of user stories covering all aspects of the feature:

1. As a <actor>, I want <feature>, so that <benefit>

Be thorough but focused on the defined scope.

## UI/UX Notes

Key screens, flows, or interactions. Describe what the user sees and does, not how it's implemented. Note any areas that need design work.

## Scope Boundaries

What is explicitly **out of scope** for this feature. Be specific to prevent scope creep.

## Dependencies

Existing systems, features, or third-party services this feature interacts with.

## Open Questions

Any unresolved decisions or questions that need further discussion.
</prd-template>
