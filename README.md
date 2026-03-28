# Agent Skills

A collection of agent skills that extend capabilities across planning, development, and tooling.

## Planning & Design

These skills help you think through problems before writing code.

- **write-a-prd** — Create a PRD through an interactive interview, codebase exploration, and module design. Filed as a GitHub issue.

  ```
  npx skills@latest add duarteplesteves/skills/write-a-prd
  ```

- **prd-to-plan** — Turn a PRD into a multi-phase implementation plan using tracer-bullet vertical slices.

  ```
  npx skills@latest add duarteplesteves/skills/prd-to-plan
  ```

- **prd-to-issues** — Break a PRD into independently-grabbable GitHub issues using vertical slices.

  ```
  npx skills@latest add duarteplesteves/skills/prd-to-issues
  ```

- **prd-to-linear-issues** — Break a PRD into Linear issues with separate design and engineering tickets per vertical slice, using the Linear MCP server.

  ```
  npx skills@latest add duarteplesteves/skills/prd-to-linear-issues
  ```

- **grill-me** — Get relentlessly interviewed about a plan or design until every branch of the decision tree is resolved.

  ```
  npx skills@latest add duarteplesteves/skills/grill-me
  ```

- **request-refactor-plan** — Create a detailed refactor plan with tiny commits via user interview, then file it as a GitHub issue.

  ```
  npx skills@latest add duarteplesteves/skills/request-refactor-plan
  ```

## Development

These skills help you write, refactor, and fix code.

- **tdd** — Test-driven development with a red-green-refactor loop. Builds features or fixes bugs one vertical slice at a time.

  ```
  npx skills@latest add duarteplesteves/skills/tdd
  ```

- **triage-issue** — Investigate a bug by exploring the codebase, identify the root cause, and file a GitHub issue with a TDD-based fix plan.

  ```
  npx skills@latest add duarteplesteves/skills/triage-issue
  ```

- **improve-codebase-architecture** — Explore a codebase for architectural improvement opportunities, focusing on deepening shallow modules and improving testability.

  ```
  npx skills@latest add duarteplesteves/skills/improve-codebase-architecture
  ```

- **qa** — Run an interactive QA session where you report bugs conversationally and the agent files GitHub issues with full codebase context.

  ```
  npx skills@latest add duarteplesteves/skills/qa
  ```

## Writing & Knowledge

- **write-a-skill** — Create new skills with proper structure, progressive disclosure, and bundled resources.

  ```
  npx skills@latest add duarteplesteves/skills/write-a-skill
  ```

- **ubiquitous-language** — Extract a DDD-style ubiquitous language glossary from the current conversation.

  ```
  npx skills@latest add duarteplesteves/skills/ubiquitous-language
  ```

- **obsidian-vault** — Search, create, and manage notes in an Obsidian vault with wikilinks and index notes.

  ```
  npx skills@latest add duarteplesteves/skills/obsidian-vault
  ```
