# Project Pulse implementation plan

## Goal

Build a small, static Project Pulse dashboard that helps contributors scan project ownership, status, recent activity, priority or risk, and a short contributor-friendly summary. Follow the project brief in `.github/project-pulse-brief.md`; use the existing custom Designer and Coder agents, coordinated by the Orchestrator.

## Ordered work and file assignments

1. **Agree on the contract** — The Orchestrator shares the brief and this plan with Designer and Coder before implementation. Use a top-level `projects` array with `name`, `owner`, `status`, `recentActivity`, and `priority` for each record; include a short `summary` to meet the contributor-friendly brief. Agree on the dashboard and card markup hooks, including `.dashboard` and `.project-card`.
2. **Implement in parallel after the contract is agreed:**
   - **Designer — `app/styles.css`:** Define the responsive visual hierarchy, readable spacing and typography, accessible contrast and focus states, and clear status/priority treatments. Style `.dashboard` and `.project-card`; use rounded corners and shadows for the polished card UI. Give Coder concise markup/accessibility guidance without editing Coder-owned files.
   - **Coder — `app/project-data.json`:** Create representative project records matching the agreed schema. Keep values readable and useful to contributors.
   - **Coder — `app/index.html`:** Create the semantic page with the exact title “Project Pulse,” link `styles.css`, load `project-data.json`, and render visible project cards from the data, including owner, status, recent activity, priority, and summary. Keep any necessary rendering logic in this assigned file; do not add unassigned app files.
   - **Coder — `.vscode/launch.json`:** Add strict JSON configuration named “Run Project Pulse Dashboard” that runs `python3 -m http.server 5500` from `${workspaceFolder}/app` and opens `http://localhost:5500/index.html`.
3. **Integrate and validate** — The Orchestrator checks that the page, data, styles, and launch configuration work together, then reports any remaining limitations.

## Responsibilities

- **Designer:** Owns the visual and interaction guidance, accessibility, responsive behavior, and `app/styles.css`. Reviews the rendered result against the dashboard brief and reports design decisions and validation recommendations.
- **Coder:** Owns implementation of `app/index.html`, `app/project-data.json`, and `.vscode/launch.json`; connects the page to the data and stylesheet, renders cards, and handles relevant loading or empty-data feedback. Validates the implementation and reports any remaining risk.
- **Orchestrator:** Establishes the shared contract, assigns the listed file ownership, resolves integration issues, and performs the end-to-end validation and handoff.

## Dependencies and parallel work

- The schema and stable markup hooks are the shared prerequisite for implementation. Agree on them first to avoid mismatches between the JSON, rendered markup, and Designer's selectors.
- Once that contract is agreed, Designer's stylesheet, Coder's data, and Coder's page/launch configuration can be developed in parallel because their file ownership does not overlap.
- Integration is sequential after those files exist: verify the page consumes the actual JSON and matching CSS hooks, then test the launch configuration against the integrated app.
- The launch setup depends on the page being at `app/index.html`; its working directory must be `app/` so the server serves the app rather than a repository directory listing.

## Edge cases and assumptions

- Handle an empty `projects` array with a clear empty state, and surface a useful message if project data cannot be loaded instead of leaving a blank dashboard.
- Keep status and priority legible without relying on color alone; ensure text remains readable at narrow viewport widths and keyboard focus is visible.
- The repository brief defines a static app and the exercise specifies Python's built-in HTTP server, so assume no framework, build step, or additional runtime dependency is needed. The `summary` field is included because the brief asks for contributor-friendly summaries even though its enumerated required fields do not list it.

## Validation expectations

- Confirm all four assigned files exist and that `app/index.html` has the “Project Pulse” title and references both `styles.css` and `project-data.json`.
- Confirm the page renders `.project-card` elements populated from the JSON, including `status`, `recentActivity`, `priority`, and the project owner and summary; check empty/error feedback if feasible.
- Parse `app/project-data.json` and `.vscode/launch.json` as strict JSON. Verify the data has a top-level `projects` array and each record has the agreed fields.
- Confirm `app/styles.css` defines `.dashboard` and `.project-card`, includes responsive and accessible treatments, and provides rounded cards and shadows.
- Start the “Run Project Pulse Dashboard” configuration and verify the browser opens `http://localhost:5500/index.html` and displays the dashboard rather than a directory listing. Stop the server after the check.
- Check the relevant automated expectations in `.github/workflows/3-step.yml`, including file presence, required references and selectors, data fields, JSON parsing, and launch name/target.
