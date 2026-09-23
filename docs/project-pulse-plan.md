# Project Pulse Implementation Plan

## Summary

Project Pulse will be a small static dashboard for contributors. It will show active projects, owners, status, recent activity, priority or risk, and a short contributor-friendly summary in a polished, responsive layout. The Planner's recommendation is to separate design decisions from implementation, keep ownership file-scoped, and integrate only after the plan and design direction are available.

## Ownership and File Assignments

| Phase | Owner | Files | Outcome |
| --- | --- | --- | --- |
| Planning | Planner, coordinated by Orchestrator | `docs/project-pulse-plan.md` | Confirm requirements, phase order, dependencies, edge cases, and validation. |
| Experience design | Designer | Design guidance for `app/index.html` and `app/styles.css`; no implementation files unless explicitly assigned | Define information hierarchy, card structure, status and priority treatment, accessibility, responsive behavior, typography, spacing, and visual direction. |
| Static implementation | Coder | `app/index.html`, `app/styles.css`, `app/project-data.json` | Build the dashboard page, polished project-card presentation, data structure, and rendering connection between the page, styles, and JSON. |
| Preview configuration | Coder | `.vscode/launch.json` | Add strict JSON for **Run Project Pulse Dashboard**, serve from `${workspaceFolder}/app`, and open `index.html`. |
| Integration and review | Orchestrator with Coder and Designer as needed | All planned outputs | Check that design decisions, markup, data, styling, and launch behavior agree; report validation and limitations. |

## Ordered Implementation Phases

### 1. Confirm the brief and constraints

The Orchestrator asks the Planner to inspect `.github/project-pulse-brief.md`, the existing agent definitions, and repository conventions. The Planner records the required fields, output files, launch behavior, and acceptance checks in this plan.

### 2. Produce the experience direction

The Designer translates the brief into a contributor-focused dashboard structure: a clear Project Pulse heading, readable project cards, visible status badges, priority or risk signals, recent activity, owners, summaries, and responsive spacing. The Designer should include accessibility considerations such as semantic structure, sufficient color contrast, meaningful labels, and a layout that remains usable on narrow screens.

### 3. Implement the static dashboard

After the design direction is available, the Coder creates:

- `app/index.html` with Project Pulse content, a stylesheet reference, the data reference, and project-card markup that exposes name, owner, status, recent activity, priority, and summary information.
- `app/styles.css` with a polished responsive layout, including `.dashboard` and `.project-card`, readable spacing, status/priority styling, `border-radius`, and `box-shadow`.
- `app/project-data.json` with a top-level `projects` array. Each project includes `name`, `owner`, `status`, `recentActivity`, and `priority`; contributor-friendly summary text may be included as an additional field.

### 4. Configure and inspect the preview

The Coder creates `.vscode/launch.json` as strict JSON with no comments. The **Run Project Pulse Dashboard** configuration must serve from the `app/` directory and open `index.html`, so the first view is the dashboard rather than a server directory listing. The Orchestrator checks that the launch target and the app's file references are consistent.

### 5. Validate and hand off

The Orchestrator reviews the four implementation files against this plan and the brief, asks the Coder to repair local defects, and records the final result, validation evidence, and known limitations in the eventual handoff. No agent stages, commits, or pushes changes; Git operations remain under the learner's control through Copilot CLI.

## Dependencies and Parallel Work

The Planner must finish before implementation assignments are finalized because the plan controls file ownership and phase order. The Designer's structure and visual decisions should be available before the Coder completes `app/index.html` and `app/styles.css`. The Coder can create representative JSON data in parallel with early design work because the required field schema is fixed by the brief, but the final data labels and card presentation must be reconciled during integration.

The Designer can work in parallel with the Orchestrator's repository inspection after the Planner has confirmed the requirements. The Coder can prepare the data model and launch configuration in parallel with the Designer's visual exploration, provided neither edits the Designer's assigned guidance. HTML/CSS integration, final launch verification, and end-to-end review are sequential after the relevant inputs exist.

## Edge Cases and Risks

- Empty or missing project data should not produce a broken layout; the implementation should make the empty state understandable or fail with a clear message.
- Long project names, owner names, activity text, and summaries must wrap without overflowing cards or changing the grid unpredictably.
- Status and priority must not rely on color alone; retain readable text or accessible labels for color-vision differences and low-contrast environments.
- The JSON must remain valid, use the required top-level `projects` array, and avoid silently mismatching property names between data and markup.
- Missing, incorrect, or relative asset paths can leave the page unstyled or data-less when served from `app/`; verify links from the configured working directory.
- `.vscode/launch.json` must remain valid JSON with no comments and must open `index.html` rather than the `app/` directory listing.
- Responsive layouts should remain legible on narrow and wide viewports, with no clipped badges, overlapping content, or unusable controls.
- Static file serving may prevent some browser behaviors when opening files directly; validate through the configured local server when possible.

## Validation Commands and Expectations

Run these checks from the repository root:

```sh
bash scripts/validate-exercise.sh
python3 -m json.tool app/project-data.json >/dev/null
python3 -m json.tool .vscode/launch.json >/dev/null
```

Also inspect the Markdown documents for the required headings, agent names, model assignments, file paths, ownership, dependencies, parallel work, edge cases, and validation language. Review `app/index.html` and `app/styles.css` for the `.dashboard` and `.project-card` hooks, visible project fields, `border-radius`, and `box-shadow`. Use the **Run Project Pulse Dashboard** launch configuration to confirm that the served first page is `index.html`; when browser inspection is unavailable, verify the launch JSON fields and referenced files directly.