# Project Pulse implementation plan

## Summary

Mona's Project Pulse dashboard is a small static web app intended to help contributors quickly review active projects, owners, status, recent activity, and priority. The implementation should follow the repository's custom agent model: the Orchestrator coordinates work, the Designer owns the visual system and CSS contract, and the Coder implements the HTML, data, and launch support files. The plan is intentionally scoped to a static dashboard with no backend or build pipeline so the team can validate the UI quickly in a VS Code Codespace environment.

This effort aligns with the exercise requirements in `.github/project-pulse-brief.md` and the agent responsibilities defined in `.github/agents/*.md`. The final goal is a dashboard that opens from `app/index.html` via a `Run Project Pulse Dashboard` launch configuration, not a directory listing.

## Ordered implementation steps

1. Confirm scope and responsibilities
   - Read the brief and agent definitions.
   - Confirm ownership boundaries: Designer owns `app/styles.css`; Coder owns `app/index.html`, `app/project-data.json`, and `.vscode/launch.json`.
   - Clarify that HTML integration follows the Designer's markup/CSS contract.

2. Define the design contract
   - Designer establishes the visual direction for a polished dashboard.
   - Define the information hierarchy, typography, color treatments, project card layout, badge states, spacing, and responsiveness.
   - Publish the contract in terms of classes and semantic structure expected by the HTML and CSS files.

3. Prepare the data contract
   - Coder defines the top-level `projects` array in `app/project-data.json`.
   - Ensure every item includes `name`, `owner`, `status`, `recentActivity`, and `priority`.
   - Confirm data values are realistic and consistent with the UI labels used by the dashboard.

4. Implement the HTML shell
   - Coder creates `app/index.html` using the Designer's markup contract.
   - The page should render a dashboard title, project cards, status badges, and other UI elements using the agreed classes and semantic structure.
   - The HTML should reference the CSS file and load the project data from the JSON file.

5. Implement the styling system
   - Designer owns `app/styles.css` and creates the final visual treatment.
   - Add deterministic hooks such as `.dashboard` and `.project-card` to make the page unmistakably Project Pulse.
   - Use shadows, rounded corners, readable spacing, badges, and responsive rules for narrow screens.

6. Add preview support
   - Coder creates `.vscode/launch.json` with the `Run Project Pulse Dashboard` launch configuration.
   - Set the working directory to `app/` and open `index.html` so the browser previews the dashboard instead of the directory root.

7. Validate the integrated dashboard
   - Check that the HTML, CSS, and JSON work together.
   - Load the app preview, verify the page displays project cards, and confirm the data-driven render behaves as expected.
   - Ensure the launch configuration opens the correct page and no server directory listing appears.

## File assignments

- `app/index.html` — Coder owns this file.
  - Purpose: static HTML shell for the Project Pulse dashboard.
  - Depends on: the Designer's markup/CSS contract.
  - Responsibility: render the dashboard using the approved class names and structure.
  - Integration rule: HTML integration follows the Designer's markup/CSS contract; Coder should not reinvent the layout or styling system.

- `app/styles.css` — Designer owns this file.
  - Purpose: dashboard styling, layout, responsiveness, color, badge states, and UI polish.
  - Responsibility: define the information hierarchy, visual affordances, and accessibility-conscious styling for the dashboard.
  - Contract: establishes the class names and design system that Coder uses when building the HTML.

- `app/project-data.json` — Coder owns this file.
  - Purpose: provide the `projects` array for the dashboard.
  - Data contract: each item contains `name`, `owner`, `status`, `recentActivity`, and `priority`.
  - Requirement: valid JSON, consistent values, and sample data sufficient to render multiple project cards.

- `.vscode/launch.json` — Coder owns this file.
  - Purpose: provide the VS Code launch configuration to preview the dashboard.
  - Requirement: run from `app/`, open `index.html`, and label the configuration `Run Project Pulse Dashboard`.
  - Avoid directory listing behavior by serving the `app/` directory context and opening `index.html` explicitly.

## Designer responsibilities

The Designer is responsible for the project's user experience and visual system, in line with `.github/agents/designer.agent.md`.

- Own `app/styles.css`.
- Define the visual hierarchy for project cards, summary blocks, status badges, and priority indicators.
- Ensure the first view clearly reads as a Project Pulse dashboard, not a generic HTML page.
- Choose a polished yet lightweight aesthetic using contrast, spacing, rounded corners, shadows, and legible typography.
- Establish responsive behavior for smaller viewports and maintain strong accessibility contrast.
- Provide the markup/CSS contract that Coder implements in HTML.
- Validate that the dashboard feels contributor-friendly, readable, and easy to scan.

## Coder responsibilities

The Coder is responsible for the implementation of the static dashboard files and support configuration, in line with `.github/agents/coder.agent.md`.

- Own `app/index.html`, `app/project-data.json`, and `.vscode/launch.json`.
- Create valid, deterministic static app files that match the Designer's contract.
- Build HTML that renders a `projects` array from `app/project-data.json` into structured project cards.
- Keep implementation simple and explicit, with no unnecessary framework or server dependency.
- Ensure the dashboard can preview directly from VS Code using the launch configuration.
- Validate JSON correctness and confirm the app opens to the dashboard page rather than a directory listing.
- Keep changes contained to the assigned files and avoid design drift outside the agreed markup contract.

## Orchestrator responsibilities

The Orchestrator, as defined in `.github/agents/orchestrator.agent.md`, runs the project coordination flow.

- Start with the Planner and use the implementation plan to create clear phases.
- Assign explicit file ownership and prevent overlap.
- Decide when parallel work is safe and when work must be sequential.
- Ensure the Designer's CSS contract is published before the Coder integrates HTML to avoid mismatched markup.
- Verify that the final dashboard is coherent across HTML, CSS, JSON, and launch configuration.
- Report blockers and validate the final result before handoff.
- Keep the process in the GitHub Copilot CLI workflow rather than collapsing everything into a single prompt.

## Dependencies

- The project brief and agent definitions must be read before implementation begins.
- The Designer's CSS and markup contract is a dependency for the Coder's HTML integration.
- The `app/project-data.json` structure is a dependency for the HTML render logic.
- The launch configuration depends on the final folder layout and app entry page.
- Validation depends on all files being present and consistent with the intended dashboard behavior.

## Parallel vs. sequential work

Explicit decisions:

- Parallel work is acceptable for the initial setup of the data contract and the design contract.
  - Designer can begin `app/styles.css` concept work while Coder prepares `app/project-data.json`.
  - This is safe because the data schema and design system are independent at first.

- Sequential work is required for integration.
  - Coder should not finalize `app/index.html` until the Designer has established the class names, layout conventions, and styling contract.
  - The launch configuration should be added after the app structure is stable but can be drafted in parallel with final polish if the page structure is already agreed.

- Final validation must be sequential and completed after all files are in place.
  - Validate JSON parsing, HTML load, CSS application, and preview behavior as a single integrated pass.

## Edge cases and error handling

- Missing or malformed `projects` data in `app/project-data.json`.
  - Handle by keeping the schema strict and validating JSON before previewing.
- Empty project list.
  - The page should show a graceful empty-state message instead of breaking the layout.
- Inconsistent status values.
  - Keep status labels deterministic and match the CSS badge styles (for example, active, at risk, paused, complete).
- Unexpected priority values or missing fields.
  - Use a small accepted set and avoid undefined rendering states.
- HTML structure that does not match the CSS contract.
  - This is a coordination issue; the Designer owns the contract and the Coder must follow it.
- Directory listing preview instead of the dashboard.
  - Prevent this by configuring `.vscode/launch.json` to serve `app/` and explicitly open `index.html`.
- Styling regressions on mobile widths.
  - Use responsive design rules and ensure readable project card stacking.

## Validation expectations

The validation pass should confirm that the dashboard is usable and the orchestrated work is integrated correctly.

- `app/project-data.json` is valid JSON and contains the expected top-level `projects` array.
- `app/index.html` exists and references the CSS file and the JSON data contract as expected.
- `app/styles.css` includes visible dashboard styling, project cards, badges, spacing, and responsive behavior.
- The page clearly reads as Project Pulse and displays project summaries in a contributor-friendly structure.
- `.vscode/launch.json` exists with the `Run Project Pulse Dashboard` configuration and points to `app/index.html`.
- The preview opens the dashboard page instead of a directory listing.
- There are no broken references between the HTML, CSS, and JSON files.

## Assumptions and open questions

Assumptions:

- This is a static dashboard, not a full app with a backend or API.
- The dashboard is a sample UI for demonstration and learning.
- A small set of fallback project entries is sufficient for the MVP.
- The environment is a Codespace or local VS Code workspace that supports the launch configuration.

Open questions:

- Should the dashboard eventually support real project data from a machine-readable source, or is this intentionally a static sample for exercise work?
- Are status labels expected to be limited to a specific vocabulary (for example, active, on track, at risk, blocked)?
- Should priority be displayed as a number, text, or both?
- Is a single-page dashboard enough, or do we need expandable details for each project in a later iteration?

## Final planning note

This plan is intentionally scoped to planning documentation only. It keeps the work aligned with the repository's custom agent definitions and the exercise expectations without modifying implementation files or performing git operations. The Orchestrator owns the workflow, the Designer owns the styling contract, and the Coder owns the code and preview support required to make Mona's Project Pulse dashboard run cleanly from the app directory.
