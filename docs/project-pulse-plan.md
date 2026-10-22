# Project Pulse implementation plan

## Goal

Build Mona's lightweight, static Project Pulse dashboard so contributors can quickly see multiple active projects, their owners, current status, recent activity, and priority or risk. The first view should be a polished, accessible, responsive dashboard—not a server directory listing. Use GitHub Copilot CLI in the Codespace to orchestrate the work with the custom Designer and Coder agents.

## Agent responsibilities

- **Orchestrator:** Ask the Planner for this plan, assign the file scopes below, sequence dependent work, avoid overlapping edits, integrate the results, and report validation and any issues.
- **Designer:** Own the visual and accessibility direction and implement `app/styles.css`. Define a clear information hierarchy, readable spacing and contrast, responsive behavior, visible status/priority treatments, and polished project cards. Provide the class/markup contract (including `.dashboard` and `.project-card`) to the Coder; do not edit files outside the assigned scope.
- **Coder:** Own application markup, sample project data, and runnable preview configuration in the files below. Implement the agreed Designer contract, render the supplied JSON data, keep errors explicit, and validate the integrated app. Do not edit the Designer-owned stylesheet unless the Orchestrator reassigns it.

## Phases and file assignments

| Phase | Owner | File assignments | Work |
|---|---|---|---|
| 1. Design contract | Designer | `app/styles.css` | Define the dashboard's responsive visual system, accessibility considerations, and selectors/markup expectations; implement polished card and badge styling. |
| 1. Data fixture | Coder | `app/project-data.json` | Create realistic contributor-friendly sample projects in a top-level `projects` array. Every project must have `name`, `owner`, `status`, `recentActivity`, and `priority`. |
| 2. Dashboard and preview | Coder | `app/index.html`, `.vscode/launch.json` | Build semantic, accessible markup titled exactly **Project Pulse**, link `styles.css`, load `project-data.json`, and render each project as a `.project-card` showing its name, owner, status, recent activity, and priority. Configure the **Run Project Pulse Dashboard** launch configuration to serve from `${workspaceFolder}/app` with `python3 -m http.server 5500`, and open `http://localhost:%s/index.html` via `serverReadyAction`. |
| 3. Integration | Orchestrator with Designer and Coder as needed | All four assigned files | Check that the HTML matches the Designer's selector contract, data keys map to rendered fields, and the launch flow opens the actual dashboard. Make only scoped fixes and rerun the relevant checks. |

## Dependencies and parallel work

- Phase 1's Designer stylesheet and Coder data fixture are independent and can run **in parallel** because they have separate file ownership.
- The Coder's `app/index.html` depends on the fixed data shape from the brief and the Designer's agreed `.dashboard`/`.project-card` and badge hooks. Start it after those contracts are established; do not have both agents edit the same file.
- `.vscode/launch.json` can be prepared alongside the markup once the `app/` serving path and `index.html` target are fixed. Integration validation must follow all Phase 1 and Phase 2 work.
- The Orchestrator owns sequencing and integration; no agent stages, commits, or pushes changes.

## Validation expectations

1. Confirm all four assigned files exist and the generated output is limited to their scopes.
2. Parse `app/project-data.json` and `.vscode/launch.json` as strict JSON (for example, `python3 -m json.tool <file>`). Confirm the data has a top-level `projects` array and each item includes `name`, `owner`, `status`, `recentActivity`, and `priority`.
3. Inspect `app/index.html` to confirm the exact **Project Pulse** title, references to `styles.css` and `project-data.json`, accessible semantic structure, and visible `.project-card` output containing status, recentActivity, and priority for every project.
4. Inspect `app/styles.css` for `.dashboard` and `.project-card` selectors, visible status/priority distinctions, `border-radius`, `box-shadow`, readable contrast and spacing, and responsive layout behavior.
5. Confirm `.vscode/launch.json` has a **Run Project Pulse Dashboard** configuration, serves from the `app` directory with `python3 -m http.server 5500`, and opens `http://localhost:%s/index.html` rather than the directory root.
6. Run the launch configuration in VS Code. Verify the browser displays the dashboard, project cards and their data load, and the app assets resolve; stop the preview server afterward.
