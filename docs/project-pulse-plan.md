# Project Pulse Planner Implementation Plan

## Goal and summary

Build a polished, accessible, responsive static dashboard that helps contributors quickly understand Mona's team projects. The dashboard will render project cards from local JSON data and be previewable through a VS Code launch configuration.

Keep the implementation dependency-free: use HTML, CSS, JSON, and a Python HTTP server. Do not add a framework or build step.

### Static app contract

- The page title and visible page heading must be exactly **Project Pulse**.
- `app/project-data.json` has a top-level `projects` array. Each project has `name`, `owner`, `status`, `recentActivity`, and `priority` fields.
- `app/index.html` references `styles.css` and loads `project-data.json`, then renders each project as a `.project-card` inside a `.dashboard` container.
- Each card presents the project's name, owner, status, recent activity, and priority.
- The launch configuration is named **Run Project Pulse Dashboard**, uses `${workspaceFolder}/app` as its working directory, serves with `python3 -m http.server 5500`, and uses `serverReadyAction` to open `http://localhost:%s/index.html`.

## Ordered implementation phases

### 1. Confirm the shared contracts

- **Orchestrator:** Confirm the data shape, HTML/CSS hooks, visible fields, and launch behavior above; communicate them to both agents before implementation.
- **Files:** No application files; this is a coordination phase.
- **Depends on:** None.

### 2. Create independent design and support artifacts

- **Designer — `app/styles.css`:** Implement the polished visual system, responsive layout, `.dashboard` and `.project-card` hooks, status and priority treatments, rounded cards, and shadows. Ensure readable contrast, spacing, and typography at narrow and wide viewport sizes.
- **Coder — `app/project-data.json` and `.vscode/launch.json`:** Create representative project data conforming to the contract, and a strict-JSON launch configuration with the required name, `cwd` set to `${workspaceFolder}/app`, server command, port, and browser-ready URL.
- **Depends on:** Phase 1. These assignments do not overlap and can proceed in parallel.

### 3. Implement the page against the agreed contracts

- **Coder — `app/index.html`:** Build semantic page structure with the exact title and visible heading; load the stylesheet and project JSON; render all required fields for every project using `.dashboard` and `.project-card`.
- **Depends on:** Phase 1. The page can be implemented alongside Phase 2 if the data schema and CSS hooks are treated as fixed; otherwise start after Phase 2 artifacts are available.

### 4. Integrate and validate

- **Orchestrator with Designer and Coder:** Review the combined page, styles, data, and launch setup; resolve integration issues within the assigned file scopes and report results.
- **Depends on:** Phases 2 and 3.

## Responsibilities

### Designer

- Own `app/styles.css` and make visual, responsive, and accessibility decisions within that scope.
- Establish clear visual hierarchy; distinguish status and priority without relying on color alone.
- Ensure cards and text remain readable without horizontal overflow across viewport sizes.
- Report design choices and validation recommendations to the Orchestrator.

### Coder

- Own `app/index.html`, `app/project-data.json`, and `.vscode/launch.json`.
- Keep the data and launch configuration deterministic and aligned with the shared contract; launch JSON must contain no comments.
- Render data rather than hard-coding card content in the page, and provide sensible feedback if project data cannot be loaded.
- Validate the assigned implementation and report any remaining risks.

### Orchestrator

- Maintain the contracts and non-overlapping ownership, coordinate dependencies and handoffs, and verify the integrated result without expanding scope.

## Dependencies and work scheduling

- `project-data.json` and `styles.css` can be created in parallel after the contract is agreed; they have separate owners.
- `index.html` depends on the JSON shape and CSS hooks, but can be implemented in parallel if those interfaces are fixed in Phase 1. Otherwise it follows Phase 2.
- `launch.json` is independent of page implementation beyond the agreed `app` location and target URL, so it can be created in parallel with both app artifacts.
- Final browser and integration validation must be sequential, after all four files exist.
- Do not have Designer and Coder edit the same file. The Orchestrator handles cross-file changes by assigning a clear owner before edits.

## Edge cases

- Empty `projects` array: show a clear empty state rather than a blank dashboard.
- Missing, malformed, or unavailable JSON: show a readable loading/error state and avoid an unhandled failure.
- Missing or empty project fields: avoid broken markup and identify incomplete information accessibly.
- Unknown status or priority values: preserve readable labels and do not depend on a fixed color mapping.
- Long project names or recent-activity text: wrap content without clipping or breaking the card layout.
- Narrow screens and zoom: cards reflow without horizontal page scrolling; content order remains understandable.
- Server port already in use or Python unavailable: report a launch/preview failure rather than treating it as a page defect.

## Validation expectations

- Parse `app/project-data.json` as JSON; confirm the top-level value contains a `projects` array and every entry has all five required fields.
- Inspect `app/index.html` for the exact `Project Pulse` title, stylesheet and JSON references, semantic heading structure, `.dashboard` container, and generated `.project-card` elements that display all five fields.
- Inspect `app/styles.css` for `.dashboard` and `.project-card`, polished card treatment including `border-radius` and `box-shadow`, accessible contrast and focus behavior where applicable, and responsive layout rules.
- Parse `.vscode/launch.json` as strict JSON; confirm the launch name, `cwd` of `${workspaceFolder}/app`, `python3 -m http.server 5500`, and `serverReadyAction` URL `http://localhost:%s/index.html`.
- Run the configured preview and confirm the browser opens `index.html` (not a directory listing), the cards render from JSON, and layout remains usable at narrow and wide widths.

## Open questions

- Which real team projects and activity details should populate the sample data? Use representative, clearly fictional examples until approved project information is available.
- Should the sample data use a prescribed vocabulary for status and priority, or should those values remain free-form strings?
- Which browser or viewport sizes should be treated as the formal responsive validation targets?
