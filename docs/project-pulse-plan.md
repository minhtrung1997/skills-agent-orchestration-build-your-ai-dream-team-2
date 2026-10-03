# Project Pulse Dashboard Implementation Plan

## Summary

Build Mona's Project Pulse as a small, dependency-free static dashboard for contributors. The dashboard will present multiple projects in accessible, responsive cards showing each project's name, owner, status, recent activity, priority, and a short contributor-friendly summary.

The implementation must create:

- `app/index.html`
- `app/styles.css`
- `app/project-data.json`
- `.vscode/launch.json`

The repository contains the Project Pulse brief, custom agent definitions, orchestration exercise instructions, and validation workflows. The application should remain plain HTML, CSS, JSON, and browser JavaScript because there is no existing frontend framework or application package manifest.

## Ordered implementation steps

### 1. Confirm requirements and assign ownership

**Owner: Orchestrator**

Read and apply:

- `.github/project-pulse-brief.md`
- `docs/agent-team.md`
- `.github/agents/planner.agent.md`
- `.github/agents/designer.agent.md`
- `.github/agents/coder.agent.md`
- `.github/steps/3-step.md`
- `.github/workflows/3-step.yml`

The Orchestrator should preserve the file boundaries below and coordinate the Designer and Coder rather than implementing the dashboard directly.

### 2. Define the dashboard experience

**Owner: Designer**

The Designer should specify the information hierarchy and visual direction before implementation:

- A clear page title with the exact text **Project Pulse**.
- A concise contributor-oriented introduction.
- A summary area that helps users understand overall project activity.
- A responsive grid or flex layout containing multiple visible project cards.
- Clear labels for owner, status, recent activity, and priority.
- Status and priority badges with sufficient color contrast and non-color text labels.
- Readable typography, spacing, focus states, and responsive behavior.
- Rounded cards and subtle shadows to create a polished dashboard rather than a plain document.
- Semantic structure and accessible markup, including heading hierarchy and appropriate ARIA only where needed.

The Designer should provide decisions and constraints to the Coder without changing files outside the assigned design scope.

### 3. Create the project data model

**Owner: Coder**

**File assignment: `app/project-data.json`**

Create valid JSON with a top-level `projects` array. Include multiple representative projects so the dashboard visibly demonstrates the card layout. Every project object must contain:

- `name`
- `owner`
- `status`
- `recentActivity`
- `priority`

Values should be concise, realistic, and contributor-friendly. Include enough status and priority variation to demonstrate the UI.

### 4. Implement the dashboard document and rendering

**Owner: Coder**

**File assignment: `app/index.html`**

Create the page shell and rendering behavior:

- Use the exact title **Project Pulse** in the document title and visible page heading.
- Link to `styles.css`.
- Load `project-data.json`.
- Render visible project cards from the `projects` data rather than hard-coding a directory listing.
- Use the class name `project-card` on every project card.
- Render each project's `name`, `owner`, `status`, `recentActivity`, and `priority`.
- Include a short contributor-friendly summary for each project, either from data or derived from the required fields.
- Use semantic elements such as `header`, `main`, `section`, `article`, `h1`, and `h2`.
- Add explicit loading and error states if the JSON cannot be loaded or is malformed; do not silently display a success-shaped empty dashboard.
- Keep browser-side logic simple and deterministic. Validate the expected top-level `projects` array and report an actionable error when validation fails.

The HTML must work when served over HTTP from the `app/` directory. It should not assume that `file://` access to JSON is permitted.

### 5. Implement the visual system and responsive layout

**Owner: Designer with Coder integration**

**File assignment: `app/styles.css`**

Implement the approved visual direction, including:

- A required `.dashboard` selector for the primary page layout.
- A required `.project-card` selector for individual project cards.
- Responsive grid behavior for narrow, medium, and wide viewports.
- `border-radius` and `box-shadow` for polished card styling.
- Clear status and priority badge treatments.
- Adequate contrast, spacing, line height, and readable content widths.
- Hover and keyboard focus states that preserve clear card identification.
- Responsive handling for long project names, activity text, and badge values.
- A reduced-motion-friendly approach if transitions or animations are included.
- Visible error and loading states matching the rest of the design.

The Designer owns visual and accessibility decisions; the Coder owns correct implementation and integration with the HTML/data contract. Any overlap must be resolved before editing so the agents do not overwrite each other's work.

### 6. Add the runnable VS Code configuration

**Owner: Coder**

**File assignment: `.vscode/launch.json`**

Create strict JSON with no comments. Add a deterministic launch configuration named exactly:

> `Run Project Pulse Dashboard`

The configuration must:

- Run `python3 -m http.server 5500`.
- Set `cwd` to `${workspaceFolder}/app`.
- Serve the application from the `app/` directory.
- Use `serverReadyAction` to open the dashboard URL: `http://localhost:%s/index.html`.
- Open `index.html`, not the server directory root, so the browser shows the Project Pulse UI rather than a directory listing.

The Coder should use a VS Code-supported launch type for a terminal command and keep the configuration self-contained; no new dependency or package installation is required.

### 7. Integrate and review

**Owner: Orchestrator**

After Designer and Coder complete their work, review all four assigned files together:

- Confirm the JSON field names match the JavaScript/HTML rendering logic.
- Confirm the HTML references the exact stylesheet and data filenames.
- Confirm every visible project card uses `project-card`.
- Confirm the CSS hooks and launch configuration match the exercise contract.
- Confirm loading and error states do not conflict with the normal dashboard layout.
- Confirm the launch configuration's working directory, command, and URL are consistent.

## File assignments

| File | Primary owner | Responsibilities |
|---|---|---|
| `app/index.html` | Coder, guided by Designer | Accessible page structure, exact Project Pulse title, data loading, project-card rendering, required visible fields, loading/error states |
| `app/styles.css` | Designer, implemented with Coder | Visual hierarchy, responsive `.dashboard` layout, `.project-card` styling, badges, spacing, contrast, focus states, `border-radius`, and `box-shadow` |
| `app/project-data.json` | Coder | Valid top-level `projects` array with `name`, `owner`, `status`, `recentActivity`, and `priority` for every project |
| `.vscode/launch.json` | Coder | Strict JSON launch configuration named `Run Project Pulse Dashboard`, serving `app/` with `python3 -m http.server 5500` and opening `index.html` |
| `docs/project-pulse-plan.md` | Planner/Orchestrator | This implementation plan; no app implementation belongs here |

## Designer responsibilities

The Designer is responsible for:

- Information architecture and visual hierarchy.
- Responsive card layout and first-view composition.
- Accessible color, typography, spacing, and contrast decisions.
- Status and priority badge semantics that do not rely on color alone.
- Loading, empty, and error-state presentation.
- Keyboard focus and interaction affordances.
- Reviewing the integrated UI for polish and usability.
- Reporting design decisions and validation recommendations without changing unassigned files.

## Coder responsibilities

The Coder is responsible for:

- Creating and wiring all assigned application files.
- Implementing deterministic data loading and rendering.
- Maintaining the JSON-to-UI field contract.
- Handling malformed data or failed requests explicitly.
- Creating `.vscode/launch.json` as strict JSON with the required command and URL.
- Preserving the Designer's layout and accessibility decisions.
- Running syntax, data, and browser-level checks before reporting completion.
- Avoiding unrelated repository changes, package installation, or framework introduction.

## Dependencies

- The Designer's layout and accessibility decisions are an input to the Coder's HTML and CSS implementation.
- `app/project-data.json` defines the data contract consumed by `app/index.html`; field names must remain identical.
- `app/index.html` depends on both `app/styles.css` and `app/project-data.json` being available at the same served root.
- `.vscode/launch.json` depends on `app/index.html` existing and on the server command being available in the Codespace.
- The final integration review depends on all three app files and the launch configuration being present.
- No npm packages, build tools, backend services, or external runtime dependencies are required.

## Parallel work decisions

The following can run in parallel after requirements are confirmed:

- Designer defines layout, accessibility, badge, and responsive behavior.
- Coder drafts `app/project-data.json`.
- Coder can prepare the launch configuration structure independently of the dashboard markup, provided it uses the fixed `app/` working directory, port `5500`, and `index.html` target.

The Designer and Coder must not concurrently edit the same file. CSS implementation should wait until the Designer's decisions are available, and HTML rendering should use the finalized JSON field contract.

The following must remain sequential:

1. Orchestrator confirms the brief, validation contract, and file ownership.
2. Designer defines the UX and accessibility direction; Coder receives those decisions.
3. Coder finalizes `app/project-data.json` before completing data-driven HTML rendering.
4. Coder implements `app/index.html` and `app/styles.css` against the agreed data and design contracts.
5. Coder creates or finalizes `.vscode/launch.json` after confirming the actual app entry point.
6. Orchestrator performs integrated review and browser validation.
7. Only after validation should the work be handed off for the next exercise step.

## Edge cases and risks

- **Missing or malformed JSON:** Show a visible, understandable error state instead of silently rendering no projects.
- **Missing required fields:** Guard against incomplete project records so one bad record does not produce broken markup or unsafe `undefined` text.
- **Empty `projects` array:** Display an intentional empty-state message.
- **Long content:** Ensure names, activity text, owners, and priority labels wrap without overflowing cards.
- **Color dependence:** Status and priority must include text labels and maintain contrast.
- **Keyboard access:** Interactive elements, if any, must have visible focus indicators and logical tab order.
- **Small viewports:** Cards must collapse to a single readable column without horizontal scrolling.
- **HTTP requirement:** Fetching JSON should be tested through the launch server, not by opening `index.html` directly from the filesystem.
- **Directory listing regression:** The launch URL must end in `/index.html`; opening only the server root is insufficient.
- **Port conflicts:** If port `5500` is already occupied, report the conflict explicitly rather than masking it with a different undocumented port.
- **Strict launch JSON:** Avoid comments or trailing commas because `.vscode/launch.json` is validated as JSON.
- **Scope drift:** Do not modify the exercise workflows, agent definitions, or unrelated repository files.

## Validation expectations

### Static validation

Run the repository's existing exercise validation where appropriate:

```bash
bash scripts/validate-exercise.sh
```

Also run focused checks for the implementation:

```bash
python3 -m json.tool app/project-data.json >/dev/null
python3 -m json.tool .vscode/launch.json >/dev/null
```

Verify that:

- `app/index.html` exists and contains the exact text `Project Pulse`.
- `app/index.html` references `styles.css` and `project-data.json`.
- `app/index.html` contains `project-card` and renders `status`, `recentActivity`, and `priority`.
- `app/styles.css` contains `.dashboard` and `.project-card`.
- `app/styles.css` contains `border-radius` and `box-shadow`.
- `app/project-data.json` contains a top-level `projects` array.
- Every project object contains `name`, `owner`, `status`, `recentActivity`, and `priority`.
- `.vscode/launch.json` contains `Run Project Pulse Dashboard`, the `python3 -m http.server 5500` command, the `app` working directory, and the `/index.html` launch URL.

### Runtime validation

Use the **Run Project Pulse Dashboard** configuration in VS Code and confirm:

1. The server starts from `app/`.
2. The browser opens `http://localhost:5500/index.html`.
3. The browser displays the Project Pulse dashboard rather than a directory listing.
4. Multiple project cards are visible.
5. Each card displays the required project fields.
6. The layout remains readable at narrow and wide viewport sizes.
7. Status and priority remain understandable without relying only on color.
8. The browser console has no errors during normal loading.
9. A failed or malformed data request produces a visible error state.

Stop the preview server after runtime validation.

## Open questions

No blocking questions remain because the repository brief fixes the required files, data fields, launch command, port, and URL. The implementation team may choose the exact visual palette, sample project content, semantic badge markup, and supported VS Code launch type, provided those choices preserve the contracts and validation requirements above.
