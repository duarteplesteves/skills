# Agent Skills

A collection of agent skills that extend capabilities across planning, development, and tooling.

## Planning & Design

These skills help you think through problems before writing code.

- **write-a-prd** — Create a lean PRD through a conversational interview, then publish it as a Linear Document under the relevant project.

  ```
  npx skills@latest add duarteplesteves/skills/write-a-prd
  ```

- **prd-to-issues** — Break a PRD into Linear issues using vertical slices, with design/engineering sub-issues, priority, and Fibonacci estimates.

  ```
  npx skills@latest add duarteplesteves/skills/prd-to-issues
  ```

- **tech-design** — Resolve open technical questions for a Linear issue before implementation, producing a decision log saved to Obsidian.

  ```
  npx skills@latest add duarteplesteves/skills/tech-design
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

## Writing & Knowledge

- **write-a-skill** — Create new skills with proper structure, progressive disclosure, and bundled resources.

  ```
  npx skills@latest add duarteplesteves/skills/write-a-skill
  ```

- **ubiquitous-language** — Extract a DDD-style ubiquitous language glossary from the current conversation.

  ```
  npx skills@latest add duarteplesteves/skills/ubiquitous-language
  ```
