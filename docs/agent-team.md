# Project Pulse Agent Team

The Project Pulse dashboard is coordinated from a GitHub Codespace using GitHub Copilot CLI. The learner selects the Orchestrator with `/agent`, then delegates planning, design, implementation, and validation to the specialist agents below. The team works on the static dashboard described in `.github/project-pulse-brief.md`.

## Agents

| Agent | Target model | Responsibility | Definition |
| --- | --- | --- | --- |
| Orchestrator | Claude Opus 4.7 (copilot) | Coordinates the workflow, delegates explicit file-scoped tasks, orders dependent phases, integrates the specialists' work, and reports the final result. | `.github/agents/orchestrator.agent.md` |
| Planner | Claude Opus 4.7 (copilot) | Researches the brief and repository, then produces the implementation plan, ownership assignments, dependencies, parallelization decisions, edge cases, and validation expectations. | `.github/agents/planner.agent.md` |
| Designer | Gemini 3.1 Pro (copilot) | Defines the dashboard information hierarchy, responsive layout, accessibility considerations, visual direction, project cards, status badges, priority treatment, and readable spacing. | `.github/agents/designer.agent.md` |
| Coder | GPT-5.5 (copilot) | Implements the static HTML, CSS, JSON data, and VS Code launch configuration within the assigned files, then validates the runnable dashboard. | `.github/agents/coder.agent.md` |

## Orchestration Context

The work takes place in the repository's GitHub Codespace and integrated terminal. The dev container installs or verifies GitHub Copilot CLI and opens it with:

```sh
copilot --allow-all --enable-all-github-mcp-tools
```

GitHub Copilot CLI is the primary interface for asking the Orchestrator to involve the Planner, Designer, and Coder. The Orchestrator keeps file scopes explicit so design guidance and implementation can be coordinated without conflicting edits. The final implementation is a small static app in `app/`, previewed through the **Run Project Pulse Dashboard** configuration in `.vscode/launch.json`.
