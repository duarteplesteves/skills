---
name: tech-design
description: Resolve open technical questions for a Linear issue before implementation, producing a lean decision log saved to Obsidian. Use when user wants to make technical decisions, resolve technical questions, plan technical approach, or do a tech design for a feature.
---

# Tech Design

Resolve open technical questions for a Linear issue by exploring the codebase, surfacing questions one at a time with recommendations, and producing a decision log.

## Prerequisites

Before starting, verify the **Linear MCP server** is configured and authenticated.
Call `mcp__linear-server__get_authenticated_user`. If it fails, tell the user:

> "The Linear MCP server is not configured. Please install and authenticate it before using this skill."

Then stop.

## Process

### 1. Accept the Linear issue

Ask the user for a Linear issue identifier (e.g., issue ID, URL, or key like `PROJECT-123`). If provided as an argument, use it directly.

Fetch the issue via `mcp__linear-server__get_issue`.

### 2. Find the PRD

From the issue, get its project via `mcp__linear-server__get_project`. Then call `mcp__linear-server__list_documents` to find the PRD document associated with that project. Fetch it via `mcp__linear-server__get_document`.

If no PRD is found, ask the user to point to one (Linear Document, local file, or paste it).

### 3. Ask for a brain dump (optional)

Before exploring the codebase, ask:

> "Before I dig into the codebase — any technical concerns or directions already on your mind?"

Accept whatever the user provides, or let them skip.

### 4. Explore the codebase

Explore the current working directory to understand the codebase's current state. Use the PRD, issue details, and brain dump to guide where you look. Focus on:

- Existing patterns and architecture relevant to the feature
- Systems and modules the feature will touch or extend
- Constraints imposed by the current implementation

### 5. Surface and resolve technical questions

Present technical questions **one at a time**. For each question:

1. State the question clearly
2. Present the options you see, with pros/cons
3. Give your **recommended answer** grounded in what you found in the codebase
4. Let the user decide — discuss until resolved

Earlier decisions inform later questions. Skip questions that become moot based on prior decisions. Aim for the key decisions that would otherwise cause flip-flopping during implementation.

### 6. Save the decision log

Once all questions are resolved, write the decision log to:

```
/Users/duarteesteves/Documents/obsidian-notes/<project-name>/decisions/<feature-name>.md
```

Where:
- `<project-name>` is the Linear project name (kebab-case)
- `<feature-name>` is derived from the issue title (kebab-case)

Create directories if they don't exist.

Present the file to the user for review. Iterate until approved, then save.

## Decision Log Template

<decision-log-template>
# {Issue title}

**Linear Issue:** {PROJECT-KEY} — {Issue title}

## Context

Brief summary of what the issue asks for and what was found in the codebase. Enough context so the decisions below make sense when re-read months later.

## Decisions

### 1. {Question title}

**Options considered:**
- **{Option A}** — {pros/cons}
- **{Option B}** — {pros/cons}

**Decision:** {Chosen option}
**Rationale:** {Why this was chosen, grounded in codebase findings.}

### 2. {Next question}

...
</decision-log-template>
