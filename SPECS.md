# Product description
AgentHub is a SaaS platform that lets companies rent pre-configured AI agents — assistants equipped with skills such as web browsing, document reading, or calendar management — for specific business tasks. This admin panel is used internally by AgentHub staff (not customers) to monitor platform health, manage client agents and contracts, maintain the skill catalog, and triage system errors.

# Tech stack & constraints
Built with HTML, Tailwind CSS (via CDN), and vanilla JavaScript only — no frameworks, no build step, no backend. All data is hardcoded directly in the HTML/JS; there are no live API calls. Light/dark mode is implemented using Tailwind's dark: variant, toggled by adding/removing a dark class on the <html> element via a top-bar switch.

# Admin panel design

## Dashboard
- Four metric cards arranged in a responsive 2×2 grid (collapsing to a single column below the md breakpoint), each displaying an icon, a label, and a hardcoded value: Total Revenue (This Month), Total Discount/Coupon Losses, Active Agents Across Clients, and Agents Flagged as Failing.
- Each metric card uses a distinct accent color tied to its metric type (e.g. green for revenue, amber for losses, blue for active agents, red for failing agents) and a subtle drop shadow to lift it off the background.
- Below the metric cards, a full-width placeholder div with a dashed border and a centered label reading "Weekly Activity Chart" marks where a chart will be implemented later.
- A persistent sidebar is visible on every view, with the current section visually highlighted (e.g. "Dashboard" highlighted while on this view).
- A toggle in the top bar switches the entire interface between light and dark mode instantly, with no page reload, by adding/removing the dark class on <html>.

## User Management
- A table listing all registered users with columns: Name, Email, Plan, and Status. Status is shown as a color-coded badge (e.g. green "Active", gray "Inactive", red "Suspended").
- Each row ends with a small action trigger — a ⋮ icon button — that opens a dropdown menu containing two options: "View detail" and "Delete". The dropdown closes when an option is selected or when clicking elsewhere on the page.
- Selecting "View detail" opens a modal overlay (dimmed backdrop, centered card) showing the full user record: name, email, plan, status, and any additional hardcoded fields (e.g. signup date, number of agents rented).
- The modal closes two ways: an explicit close button (✕) in the top-right corner of the modal, and clicking anywhere on the dimmed backdrop outside the modal card.
- Selecting "Delete" removes the corresponding row from the table (client-side only, no confirmation dialog required since this is a static prototype).

## Agent Management
- A table listing all registered agents with columns: Agent Name, Owner (client name), Status, and Skills. Status is shown as a color-coded badge (active = green, inactive = gray, failing = red).
- The Skills column shows a collapsed summary by default (e.g. "3 skills" or the first skill name + "+2 more") with a small expand control (chevron icon) next to it. Clicking the chevron reveals the full list of skill names inline, animated with a smooth height/opacity transition rather than appearing instantly. Clicking again collapses it back.
- Each row ends with a ⋮ action dropdown (the same shared component from User Management) containing two options: "Configure" and "Delete".
- Selecting "Configure" opens a modal showing the agent's system prompt in a read-only, monospace-font text block, along with the agent's name and current status as a header.
- The modal closes via an explicit close button and by clicking the backdrop, consistent with the User Management modal behavior.
- Selecting "Delete" removes the corresponding row from the table (client-side only).

## Skills
- A catalog view listing each skill as a card or row, showing: skill name, a short description (1–2 sentences), and a count indicator of how many agents currently have that skill attached (e.g. a small badge reading "12 agents").
- A brief in-panel explanation block (e.g. a small info banner or subtitle at the top of the section) defining what a "skill" means in this platform context — a capability, such as web browsing, document reading, or calendar management, that can be attached to an agent to extend what it can do.
- Each skill has a ⋮ action dropdown (the shared component) containing two options: "View detail" and "Delete".
- Selecting "View detail" opens a modal showing the skill's full description, its name as the header, and the list of agents currently using it.
- The modal closes via an explicit close button and by clicking the backdrop, consistent with prior sections.

## Agent Contracts
- A table listing all active and past rental contracts with columns: Client, Rented Agent, Contracted Skills (shown as a comma-separated list or small inline badges), Contract Dates (start–end), and Total Amount Paid.
- Past contracts are visually distinguished from active ones — e.g. active rows use normal text weight while past contracts are slightly muted/grayed out, or a "Status" badge (Active/Expired) is added to each row.
- Each row ends with a ⋮ action dropdown (the shared component) containing one option: "View detail".
- Selecting "View detail" opens a modal showing the full contract breakdown: client name, agent name, contract dates, and an itemized list of each contracted skill with its individual price, ending in a total that matches the row's displayed amount.
- The modal closes via an explicit close button and by clicking the backdrop, consistent with prior sections.

## Error Log
- A table (or stacked log entries) listing execution errors with columns: Timestamp, Agent Name, Error Type, and a short Description.
- Error Type and/or severity is shown as a color-coded badge — e.g. red for critical, amber for warning, gray for info — distinct from the status badges used elsewhere in the panel so the two badge systems aren't visually confused.
- Each entry ends with a ⋮ action dropdown (the shared component) containing two options: "View detail" and "Mark as resolved".
- Selecting "View detail" opens a modal showing the full error trace (a longer, monospace-font text block) along with the timestamp, agent name, and error type as header context.
- Selecting "Mark as resolved" updates the entry's visual state in the table — e.g. the row becomes grayed out or a "Resolved" badge replaces the error-type badge — without removing it from the log (errors should stay visible as a historical record, unlike the delete behavior elsewhere).
- The modal closes via an explicit close button and by clicking the backdrop, consistent with prior sections.

# Component inventory
- Sidebar — persistent navigation visible on every view, listing all six sections, with the currently active section visually highlighted.
- Metric Card — used on the Dashboard; displays an icon, label, and hardcoded value, with a distinct accent color per metric type and a subtle shadow.
- Action Dropdown (⋮ menu) — a small button that opens a contextual menu of actions (varies by section: View detail, Delete, Configure, Mark as resolved); closes on selection or on outside click.
- Modal Overlay — a centered card on a dimmed backdrop, used for all "View detail," "Configure," and error-trace views; closes via an explicit ✕ button or by clicking the backdrop.
- Status/Severity Badge — a small color-coded pill used to indicate user status, agent status, error severity, and contract status; color mapping is consistent within each context but distinct between status-type and severity-type badges.
- Collapsible List — used for an agent's skill list; toggled by a chevron control with a smooth expand/collapse transition.
- Dark Mode Toggle — a top-bar switch that adds/removes the dark class on <html>, instantly re-themeing the entire interface using Tailwind's dark: variant.

# Acceptance criteria
- Clicking the dark mode toggle switches the entire interface between light and dark themes instantly, with no page reload, and the toggle state is visually reflected (e.g. icon or switch position changes).
- Clicking any row's ⋮ action dropdown opens a menu of contextual options; clicking outside the menu closes it without taking action.
- Selecting "View detail" from any section opens a modal displaying that row's full hardcoded data, matching what's shown in the table/card.
- Clicking a modal's close button (✕) closes the modal and returns focus to the underlying view.
- Clicking the dimmed backdrop outside a modal also closes it, with identical behavior to the close button.
- Clicking the expand chevron on an agent's collapsed skill list reveals the full skill names with a smooth transition; clicking again collapses it back.
- Selecting "Delete" on a User Management, Agent Management, or Skills row removes that row from its table/list.
- Selecting "Mark as resolved" on an Error Log entry visually updates that entry's state (e.g. grayed out or badge change) without removing it from the log.
- The sidebar is visible and identical across all six sections, with the active section always visually highlighted to match the currently displayed view.
- All data displayed (metrics, users, agents, skills, contracts, errors) is hardcoded in the HTML/JS — no network requests are made at any point.
