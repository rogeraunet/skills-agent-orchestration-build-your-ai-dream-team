# Project Pulse implementation plan

## Summary

Build Mona's lightweight, static Project Pulse dashboard so contributors can scan active projects, owners, status, recent activity, priority or risk, and a short contributor-friendly summary. Keep the implementation to the requested HTML, CSS, JSON, and VS Code launch configuration; the repository has no existing `app/` implementation or app-specific build/test framework.

Use the prebuilt custom agents and their existing boundaries: the Orchestrator coordinates and integrates, Designer owns user-experience and styling decisions, and Coder owns the runnable implementation. Follow the project brief in `.github/project-pulse-brief.md` and the later exercise requirements in `.github/steps/3-step.md`; keep learner outputs untracked by default, as described in the exercise outline.

## Responsibilities and file assignments

| Agent | Assigned files | Responsibility |
|---|---|---|
| **Designer** | `app/styles.css` | Define the visual hierarchy, card layout, responsive behavior, readable spacing and contrast, accessible focus/contrast treatment, and status/priority affordances. Provide the HTML-facing design guidance needed by Coder. Include deterministic `.dashboard` and `.project-card` selectors; use polished card treatments including `border-radius` and `box-shadow`. Do not edit Coder-owned files. |
| **Coder** | `app/index.html`, `app/project-data.json`, `.vscode/launch.json` | Implement accessible, semantic dashboard markup and render project cards from the JSON data; provide representative project records; and create the strict-JSON VS Code launch configuration that serves the `app/` directory and opens the dashboard page. Reference Designer's CSS hooks and decisions without changing the stylesheet. |
| **Orchestrator** | No implementation files | Ask Planner for this plan first, then assign the bounded file scopes above, sequence dependent work, integrate the Designer and Coder results, and report validation and any limitations. Keep all work within the requested files. |

## Ordered implementation steps

### 1. Confirm the shared contract

- **Owner:** Orchestrator, with Designer and Coder acknowledging the contract.
- **Files:** none.
- Confirm the brief and required UI/data/launch behavior before implementation. Use `projects` as the top-level JSON array; every project record has `name`, `owner`, `status`, `recentActivity`, and `priority`. The UI must show these values and a short contributor-friendly summary.
- Agree that `project-card` is the card class and `.dashboard` / `.project-card` are stable CSS hooks. Keep data local and static; do not introduce a framework, package manifest, or build step.

### 2. Produce design and project data in parallel

- **Designer:** Create `app/styles.css` using the agreed hooks and responsive, accessible card-based layout.
- **Coder:** Create `app/project-data.json` with a top-level `projects` array and multiple realistic, concise sample records that exercise status and priority/risk presentation.
- These tasks can run **in parallel** after the shared contract is agreed: the stylesheet and JSON data are separate files and neither requires the other's implementation. Designer should tell Coder the intended layout and semantic hooks before HTML is finalized.

### 3. Implement and connect the page and launch configuration

- **Coder:** Create `app/index.html` and `.vscode/launch.json`.
- The HTML depends on the data schema from Step 2 and the Designer's style hooks/layout guidance. Use the exact page title `Project Pulse`; reference `styles.css` and `project-data.json`; render visible `project-card` elements populated from the JSON, including status, `recentActivity`, priority, and summary. Include semantic landmarks/headings, useful accessible names, and a readable empty/error state if data cannot be loaded.
- Configure the launch entry with the exact name **Run Project Pulse Dashboard**. Use `python3 -m http.server 5500`, set the working directory to `${workspaceFolder}/app`, and configure `serverReadyAction` to open `http://localhost:%s/index.html`. This serves the app root and opens the HTML page rather than a directory listing.
- The launch file can be drafted in parallel with HTML once the agreed server command and app path are known, but final validation of it is part of integration in Step 4.

### 4. Integrate and validate

- **Owner:** Orchestrator; Coder fixes implementation issues in Coder-owned files and Designer fixes styling issues in Designer-owned files.
- Review the assembled page against the brief, confirm that file references and CSS hooks agree, then run static checks and launch the preview. Resolve cross-file issues through the Orchestrator so file ownership remains clear.

## Dependencies and execution order

- The shared requirements/schema in Step 1 precede all implementation.
- Step 2's CSS and JSON work is independent and can proceed **in parallel**.
- HTML implementation in Step 3 is **sequential after** the Step 1 contract and should use the completed JSON field names and Designer's CSS hooks/design direction.
- The launch configuration can be authored alongside the HTML after Step 1 because it has no data or styling dependency; however, it must be tested only after the app files exist.
- Integrated validation is **sequential after all four files are present**. Any corrections should go to their assigned file owner, followed by the relevant checks again.

## Validation expectations

1. **File and content checks:** Confirm all four assigned files exist. Confirm `app/index.html` has the exact `Project Pulse` title, references `styles.css` and `project-data.json`, and renders `project-card` elements showing project name, owner, status, recent activity, priority, and summary. Confirm CSS has `.dashboard`, `.project-card`, responsive behavior, `border-radius`, and `box-shadow`.
2. **Data checks:** Run `python3 -m json.tool app/project-data.json`; inspect that the top-level `projects` value is an array and each record includes `name`, `owner`, `status`, `recentActivity`, and `priority`.
3. **Launch checks:** Run `python3 -m json.tool .vscode/launch.json`; inspect that it is strict JSON, names the configuration `Run Project Pulse Dashboard`, uses `python3 -m http.server 5500`, serves with `cwd` `${workspaceFolder}/app`, and opens `http://localhost:%s/index.html` through `serverReadyAction`.
4. **Runtime and accessibility review:** Start the launch configuration from VS Code Run and Debug. Verify the browser shows the dashboard rather than a directory listing, cards load from JSON, the layout remains usable at narrow widths, and keyboard navigation, headings, contrast, and status/priority meaning remain understandable. Stop the preview server afterward.
5. **Repository checks:** Run `scripts/validate-exercise.sh` when available in the exercise environment. Its checks cover existing repository conventions and required output paths; it is not a substitute for the runtime preview and responsive/accessibility review.

## Open questions and assumptions

- The brief does not prescribe project names, statuses, or a color palette; use clear representative sample data and let Designer select an accessible visual system.
- No application framework or test runner is present or needed. Use the requested static files and Python's standard-library HTTP server for preview.
