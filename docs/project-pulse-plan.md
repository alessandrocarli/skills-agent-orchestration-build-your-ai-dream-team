# Project Pulse Dashboard Implementation Plan

## Summary

Build Mona's Project Pulse as a small, static, contributor-friendly dashboard. The dashboard will display multiple projects with their name, owner, current status, recent activity, and priority or risk level. It must use semantic HTML, accessible labels and contrast, responsive card-based styling, and deterministic selectors required by the repository checks.

The implementation is intentionally dependency-free. The app will consist of:

- `app/index.html`
- `app/styles.css`
- `app/project-data.json`
- `.vscode/launch.json`

The repository is a minimal exercise template. There is no existing frontend framework, package manifest, build system, or application source to preserve. Existing conventions are plain files, strict JSON, GitHub Copilot CLI orchestration, and validation through `scripts/validate-exercise.sh` plus the Step 2 and Step 3 workflow checks.

## Responsibilities

### Orchestrator

- Use this plan to create implementation phases and assign explicit file ownership.
- Give Designer and Coder the same markup and CSS contract before parallel work begins.
- Keep overlapping file scopes in separate phases.
- Integrate and review the completed dashboard rather than implementing files directly.
- Confirm that the launch configuration serves the `app` directory and opens `index.html`, not a directory listing.
- Coordinate final validation and prepare the handoff for the next exercise step.

### Planner

- Define the file ownership, data contract, visual contract, dependencies, edge cases, and validation commands in this document.
- Confirm requirements against `.github/project-pulse-brief.md`, `.github/agents/`, `.github/workflows/2-step.yml`, `.github/workflows/3-step.yml`, and `scripts/validate-exercise.sh`.
- Do not modify application files or launch configuration.

### Designer

**Assigned file:** `app/styles.css`

- Define the visual hierarchy for the Project Pulse title, summary, project grid, project cards, status badges, and priority treatment.
- Create a polished dashboard rather than a bare HTML page.
- Include the required `.dashboard` and `.project-card` selectors.
- Use rounded cards, shadows, readable spacing, clear typography, and responsive layout behavior.
- Ensure statuses and priorities are visually distinct without relying on color alone.
- Provide accessible focus, hover, and reduced-motion behavior where applicable.
- Use selectors that match the agreed HTML contract:
  - `.dashboard`
  - `.dashboard-header`
  - `.project-grid`
  - `.project-card`
  - `.project-card__header`
  - `.project-card__meta`
  - `.status-badge`
  - `.priority-badge`
  - `.activity`
- Keep the stylesheet self-contained and dependency-free.

### Coder

**Assigned files:**

- `app/index.html`
- `app/project-data.json`
- `.vscode/launch.json`

- Implement semantic, accessible HTML for the dashboard.
- Include the exact visible title `Project Pulse`.
- Link `styles.css` from `index.html`.
- Load or reference `project-data.json` from `index.html`.
- Render visible project cards using the `project-card` class.
- Render each project's `status`, `recentActivity`, and `priority` values in the UI.
- Use a top-level `projects` array in `project-data.json`.
- Include `name`, `owner`, `status`, `recentActivity`, and `priority` on every project object.
- Create `.vscode/launch.json` as strict JSON with no comments.
- Add a configuration named `Run Project Pulse Dashboard`.
- Set the launch working directory to `${workspaceFolder}/app`.
- Run `python3 -m http.server 5500`.
- Configure `serverReadyAction` to open `http://localhost:%s/index.html`.
- Keep the launch configuration deterministic and ensure it opens the dashboard frontend rather than the server directory listing.

## Agreed implementation contract

### HTML and rendering

`app/index.html` should:

- Use a meaningful document title containing `Project Pulse`.
- Include a semantic `<main>` element with the `.dashboard` class.
- Include a dashboard header with the product title and a short contributor-friendly description.
- Include a project region with an accessible heading.
- Render a card grid using `.project-grid`.
- Render one `.project-card` per project.
- Give each card a clear heading for the project name.
- Show the owner, status, recent activity, and priority using visible labels.
- Use appropriate semantics such as headings, lists, `<dl>`, or labeled sections.
- Avoid hard-coded project content as the only source of truth; the page should load `project-data.json` and render the project array.
- Handle a failed data request with a visible, user-facing error message instead of silently showing an empty dashboard.
- Keep the data-loading logic inline if no separate JavaScript file is assigned. Do not introduce an unassigned application file.

### Data

`app/project-data.json` should contain valid JSON in this shape:

```json
{
  "projects": [
    {
      "name": "Example project",
      "owner": "Contributor name",
      "status": "Active",
      "recentActivity": "Short activity summary",
      "priority": "High"
    }
  ]
}
```

Use several representative projects so the dashboard demonstrates the card layout. Values should be concise, contributor-friendly, and consistent enough for status and priority styling. The required property names must be preserved exactly.

### Styling

`app/styles.css` should include:

- A clear page background and readable default typography.
- A responsive `.dashboard` container with sensible maximum width and horizontal padding.
- A responsive `.project-grid` that changes from one column on narrow screens to multiple columns when space permits.
- `.project-card` styling with `border-radius` and `box-shadow`.
- Consistent spacing between card sections.
- Status and priority badges with sufficient contrast.
- A visual distinction between normal, elevated, and urgent priority values without depending exclusively on color.
- Visible keyboard focus states.
- Mobile-safe text wrapping and no intentional horizontal overflow.
- A media query for narrow viewports.
- Optional reduced-motion handling for transitions.

### Launch configuration

`.vscode/launch.json` must be valid strict JSON. The planned configuration should use the Python HTTP server from the `app` directory and open the explicit file URL:

- Name: `Run Project Pulse Dashboard`
- Working directory: `${workspaceFolder}/app`
- Command: `python3 -m http.server 5500`
- URL pattern: `http://localhost:%s/index.html`

A `node-terminal` launch configuration is suitable for this repository because it can run the command with a working directory and use `serverReadyAction`. The exact VS Code launch schema should be checked against the installed VS Code behavior, but comments and JSON trailing commas are not permitted.

## Ordered implementation steps

### 1. Confirm requirements and contracts

**Owner:** Orchestrator, with Planner input  
**Files:** No application changes

- Read `.github/project-pulse-brief.md`.
- Read `.github/agents/planner.agent.md`, `designer.agent.md`, `coder.agent.md`, and `orchestrator.agent.md`.
- Read the Step 2 and Step 3 workflow checks.
- Confirm the exact required selectors, data keys, launch name, command, working directory, and URL.
- Give Designer and Coder the agreed contract above.

**Dependency:** This is the prerequisite for all implementation work.

### 2. Produce visual and accessibility direction

**Owner:** Designer  
**File:** `app/styles.css`

- Implement the complete responsive visual system.
- Ensure the required deterministic selectors and polished card treatments are present.
- Keep the CSS aligned with the agreed HTML contract so Coder can implement HTML without guessing selector names.

**Dependency:** Requires Step 1's selector and semantic contract.

### 3. Implement dashboard structure and data

**Owner:** Coder  
**Files:** `app/index.html`, `app/project-data.json`

- Create the semantic document shell and dashboard content.
- Load `project-data.json`.
- Render project cards and all required fields.
- Add visible loading and error states.
- Ensure the HTML class names match the Designer's CSS contract.

**Dependency:** Requires Step 1. This can proceed in parallel with Step 2 because the markup and selector contract is fixed in advance.

### 4. Implement runnable preview support

**Owner:** Coder  
**File:** `.vscode/launch.json`

- Create strict JSON with the named launch configuration.
- Set `cwd` to `${workspaceFolder}/app`.
- Run `python3 -m http.server 5500`.
- Open `http://localhost:%s/index.html` through `serverReadyAction`.

**Dependency:** Requires the agreed app location but does not require the final visual styling. It can proceed in parallel with Steps 2 and 3, provided the command and port are fixed.

### 5. Integrate and review

**Owner:** Orchestrator, Designer, and Coder  
**Files:** All four assigned files

- Review the rendered page against the CSS selectors and data fields.
- Confirm the dashboard is not a directory listing.
- Confirm cards display actual values from `project-data.json`.
- Check that responsive behavior, status badges, priority treatment, contrast, and focus states work together.
- Resolve any cross-file mismatch sequentially after the parallel implementation work is complete.

**Dependency:** Must occur after Steps 2–4.

### 6. Validate and hand off

**Owner:** Orchestrator  
**Files:** All four assigned files

- Run the repository validation commands.
- Start the preview server or use the VS Code launch configuration.
- Check the explicit `/index.html` URL.
- Stop the server after validation.
- Report files changed, validation results, and any remaining risk.

**Dependency:** Must occur after integration.

## Parallel versus sequential work

### Can proceed in parallel

After the contract in Step 1 is established:

- Designer can implement `app/styles.css`.
- Coder can implement `app/index.html`.
- Coder can create `app/project-data.json`.
- Coder can create `.vscode/launch.json`.

These tasks have non-overlapping file scopes. The parallel work is safe only if both agents use the agreed selectors, exact data keys, launch name, command, port, working directory, and URL.

### Must proceed sequentially

- Repository research and contract definition must precede implementation.
- Integration review must follow both Designer and Coder work.
- Browser/launch validation must follow integration.
- Any correction involving both HTML and CSS must be handled sequentially to prevent one agent from overwriting the other agent's assumptions.
- Final handoff must follow validation.

## Dependencies and risks

- `index.html` depends on the exact data keys in `project-data.json`.
- `index.html` and `styles.css` depend on the shared selector contract.
- The fetch-based data load requires HTTP serving; opening `index.html` directly from the filesystem may be blocked by browser cross-origin rules.
- `launch.json` must serve from `app` and explicitly open `index.html`; opening the workspace root risks showing a directory listing.
- Port `5500` may already be in use. If this occurs, stop the conflicting process before validation rather than silently changing the planned port.
- JSON must not contain comments or trailing commas.
- Status and priority values may vary in capitalization. Rendering and styling should either use consistent fixture values or normalize values before assigning classes.
- A failed data request must be visible and understandable.
- Empty or malformed project data should not produce an unexplained blank page.
- Accessibility should not depend on color alone, especially for status and priority.

## Validation expectations

Run these checks from the repository root.

### Plan-file checks

```bash
test -f docs/project-pulse-plan.md
grep -Eiq 'Project Pulse|Designer|Coder|dependencies|parallel|validation' docs/project-pulse-plan.md
grep -Fq 'app/index.html' docs/project-pulse-plan.md
grep -Fq 'app/styles.css' docs/project-pulse-plan.md
grep -Fq 'app/project-data.json' docs/project-pulse-plan.md
grep -Fq '.vscode/launch.json' docs/project-pulse-plan.md
```

### JSON checks

```bash
python3 -m json.tool app/project-data.json >/dev/null
python3 -m json.tool .vscode/launch.json >/dev/null
```

Confirm manually or with a JSON-aware check that:

- `project-data.json` has a top-level `projects` array.
- Every project has `name`, `owner`, `status`, `recentActivity`, and `priority`.
- `launch.json` contains `Run Project Pulse Dashboard`.
- `launch.json` contains `${workspaceFolder}/app`.
- `launch.json` contains `python3 -m http.server 5500`.
- `launch.json` opens `http://localhost:%s/index.html`.

### Repository exercise validation

```bash
bash scripts/validate-exercise.sh
```

This existing script checks the repository's JSON, shell, workflow, learner-file, and Project Pulse exercise requirements. Run it after the complete exercise output exists; it is not a substitute for the browser smoke test.

### Static content checks

```bash
grep -Fq 'Project Pulse' app/index.html
grep -Fq 'styles.css' app/index.html
grep -Fq 'project-data.json' app/index.html
grep -Fq 'project-card' app/index.html
grep -Fq 'status' app/index.html
grep -Fq 'recentActivity' app/index.html
grep -Fq 'priority' app/index.html
grep -Fq '.dashboard' app/styles.css
grep -Fq '.project-card' app/styles.css
grep -Fq 'border-radius' app/styles.css
grep -Fq 'box-shadow' app/styles.css
```

### Browser and launch smoke test

Use **Run and Debug** and select **Run Project Pulse Dashboard**. Confirm that:

- The server starts from `app`.
- The browser opens `http://localhost:5500/index.html`.
- The page shows the Project Pulse title.
- Multiple project cards are visible.
- Owner, status, recent activity, and priority are visible on each card.
- The page has polished spacing, card rounding, shadows, and responsive behavior.
- No directory listing is shown.
- The error state can be displayed if the data request fails.

A command-line smoke test can also be used:

```bash
python3 -m http.server 5500 --directory app >/tmp/project-pulse-server.log 2>&1 &
server_pid=$!
trap 'kill "$server_pid" 2>/dev/null || true' EXIT
sleep 1
curl -fsS http://localhost:5500/index.html | grep -Fq 'Project Pulse'
curl -fsS http://localhost:5500/project-data.json | python3 -m json.tool >/dev/null
```

## Open questions

- No open product questions block implementation. The brief specifies the required fields, files, launch behavior, and visual direction.
- The exact VS Code launch `type` may depend on the available built-in debugger. Prefer a built-in configuration that supports `node-terminal` and `serverReadyAction`; validate it in the Codespace.
- No additional dependencies should be introduced unless the launch environment proves unable to support the dependency-free Python HTTP server approach.
