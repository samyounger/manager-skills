---
name: sprint-planner
description: Plan the next sprint from release commitments, backlog readiness, team availability and recent delivery. Use for sprint planning, capacity checks, publishing goal-ready notes or delivery forecasting; not for retrospectives or changing work-management data without approval.
metadata:
  version: 1.4.0
---

# Sprint Planner

Create a credible, delivery-led plan for the next sprint. The plan should make release commitments, active work, upcoming work, capacity, dependencies and confidence visible early enough to reduce scope or resolve a risk before the sprint begins.

Do not create or edit goals, sprints, tickets, boards, calendars or other work-management data unless the user explicitly asks. Draft the plan first. Follow the workspace's required structured-content format when changing work-management descriptions, comments or custom fields.

The default publishable artefact is a set of local Markdown sprint-goal notes in the configured knowledge-base directory. These are reviewable, portable copy for a person to paste into any goal-sharing tool. Do not create external goals from the notes unless the user separately and explicitly authorises that action.

Write all prose in the user's documented language and spelling preference.

## Establish local planning context

Read `.agents/em-context.md` first. For this skill, use its **Delivery planning system** section, which should hold the durable, organisation-specific configuration that does not belong in a reusable skill:

- roadmap/release source and the exact inclusion criterion;
- delivery-team rosters and planned capacity roles, team backlogs or boards, team leave calendars and any other capacity source;
- goal hierarchy, naming convention, goal-template links and sprint duration/boundary rule;
- local sprint-goal publication directory and whether an external goal tool is a manual publishing destination;
- the authoritative system connector and required content format;
- known holding epics, cross-cutting programmes, working patterns and local public-holiday assumptions.

Do not hard-code an organisation, team, product, URL, board, project key or work-item type in this skill. If the delivery-planning section is missing or lacks a material input, ask for it, then add the durable fact to `.agents/em-context.md` before preparing a detailed plan. Do not save a one-off absence, a proposed scope change, or a temporary risk as durable context.

Use the structured delivery-team roster in the **Delivery Planning System** section to populate the matching note's `individualContributors` and `teamLeads` Dataview arrays. Copy every person and role exactly; set each `days` value from the current sprint's calendar or approved plan. Verify roster changes against the latest approved team plan or with the user. Retrieve PTO, public holidays and other sprint-specific availability from the relevant calendar or approved plan; never reuse a prior sprint's absence days as current capacity.

Use the configured system's native connector or MCP for retrieving and changing its data. Do not use a browser as a substitute when a configured connector is available.

For a parent-plus-team-goal hierarchy, read [the goal template](references/goal-template.md). For calculations and forecast rules, read [metrics and capacity](references/metrics-and-capacity.md). Use [external sources](references/sources.md) only when their supporting rationale is relevant.

## Plan the sprint

### Establish the sprint boundary

- Find the latest completed sprint record and increment its configured naming sequence.
- Start the new sprint on the configured boundary, normally the calendar day the prior sprint completed. Calculate the configured sprint duration from that date.
- Validate the prior completion date against the board or sprint record if the goal dates are ambiguous; goal creation and due dates are not automatically sprint boundaries.
- List the resulting start, end, working days and relevant public holidays. Ask one targeted question if the completion date or a person's working pattern cannot be established.

### Convert demand into a feasible commitment

Start from the configured roadmap/release source, then work down into deliverable backlog items. For every roadmap-supported release, record its required release or readiness date, must-have outcome, linked work, responsible team, delivery owner, test/release readiness, dependency owner and consequence of slipping. A release date is not proof that work is ready.

Build each team's candidate list in this order:

1. Committed release or release-readiness work needed within or immediately after the sprint.
2. Already-started work whose next outcome unblocks a release, customer value or another team.
3. Maintenance, reliability, bugs, security and migration work with an explicit operational outcome.
4. Upcoming/discovery work that reduces uncertainty or creates a genuinely ready option for a later sprint.

For each candidate, check a clear end state, acceptance/quality path, accountable owner, dependency and enough work ready to start. Keep discovery separate from delivery: state the decision or artefact it will produce, not a vague intention to “make progress”. Do not commit materially blocked work, unowned work or work with no credible path through verification without labelling it as a risk or placing it in upcoming work.

Use three commitment lanes:

- **Must deliver:** the small set of outcomes that protects the roadmap/release plan. Each needs a release/verification date and a recovery or scope-cut path.
- **Planned delivery:** active, ready work that fits evidence-based capacity after must-deliver work and operating load.
- **Pull/contingency:** ready, low-dependency work that starts only when the committed lanes are on track. It is not part of the forecast.

Prefer a smaller committed outcome over a broad promise with unclear testing, approval, rollout or dependency ownership. Where release demand exceeds forecast capacity, name the decision required: reduce scope, move a lower-priority item, bring a dependency forward, or explicitly accept a date risk. Never inflate velocity or use a single heroic sprint as the baseline.

### Calculate and use capacity

Calculate capacity separately for each delivery team. Use people-days rather than nominal headcount:

`available people-days = sum(each contributor's scheduled working days) − PTO − public holidays − known non-delivery commitments`

Show the team roster/role, absences, individual available days, baseline people-days, lost days, available people-days and availability percentage. Do not count a lead, quality engineer, product partner or shared specialist as full development capacity unless their actual contribution is known; show it separately. Apply a sensible contingency to the delivery commitment (normally around 10%, adjusted for recent unplanned work and delivery risk), and show it explicitly.

Forecast with each team's own recent performance, adjusted for new availability:

`adjusted point forecast = trailing median completed story points × (new available people-days / historical median available people-days)`

`adjusted throughput forecast = trailing median completed ticket count × (new available people-days / historical median available people-days)`

Use the lower credible forecast if a material release dependency, unusually high bug load, team change or migration makes the normal run-rate optimistic. Point and ticket-count forecasts are complementary: do not convert tickets to points and never compare velocity between teams.

## Draft and publish the planning tree

Where the configured model uses a parent goal and child team goals, draft one parent note and one note per delivery team. The parent description is concise natural-language text covering the combined externally meaningful outcome, risk reduction, overall capacity constraint and any material decision or risk. Follow it with **Sprint focus** and **Capacity and dependencies** when they improve scanability. It is not a concatenation of team task lists.

Use the configured live goal format, guided by [the goal template](references/goal-template.md). Each team note is a portfolio view, not a ticket dump: represent every project in its relevant lane as a linked Jira epic or initiative followed by a short, nested objective and named lead. The objective must state the meaningful sprint end state: a percentage/milestone of the epic, release readiness, or verification. Do not invent a completion percentage or confidence label; state delivery risk in the objective or parent dependency summary when the evidence supports it. Write `N/A` when Planning / Upcoming or Discovery has no work. Keep team allocation out of the parent but explicit in the appropriate team note.

An initiative may appear in more than one team note when the teams have distinct, named contributions (for example, a shared release gate or programme milestone). Make each team's objective and lead specific; do not present the same full commitment twice.

When the user asks to publish the plan, create the configured local notes with Obsidian-compatible frontmatter. Add wikilinks between the parent and team notes when they share a directory, and retain normal Markdown links for external systems. Use compact tables for capacity figures and absences rather than dense capacity bullet lists. Mark the notes as `ready-to-publish` unless a material evidence gap means they need review. Do not make the ability to write an external goal a prerequisite for publishing the local artefact.

Before proposing the plan, run this quality gate:

- Every roadmap release in the sprint window that depends on the teams is represented, explicitly deferred or marked at risk.
- Each must-deliver outcome has an owner, measurable end state, verification/release path and dependency status.
- Each team’s committed points and ticket count fit its adjusted forecast and capacity after contingency.
- Cross-team initiatives have distinct team contributions; hand-offs have a named receiving owner and date.
- Maintenance, bugs and cross-cutting programme capacity are visible rather than absorbed into an implausible feature commitment.
- The parent says what the organisation gets; the team goals say how each team will deliver it.

## During and after the sprint

At planning, record baseline commitments separately from anything added later. At sprint end, record completed-at-close measures separately for each team: committed/completed story points, committed/completed ticket count, spillover count and reason, unplanned ticket count, work-type mix, people-days, and whether each must-deliver/release outcome landed. Use the trend to improve the next forecast; never use it to rank individuals or teams.

Call out a meaningful change: a sustained fall in availability-adjusted throughput, repeated spillover at the same stage, a release repeatedly missing readiness work, or a growing unplanned-work share. Investigate the constraint (scope, dependencies, quality, support load or workflow) before changing the velocity target.
