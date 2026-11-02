---
name: sprint-planner
description: Plan the next sprint from release commitments, backlog readiness, team availability and recent delivery. Use for sprint planning, capacity checks, publishing goal-ready notes or delivery forecasting; not for retrospectives or changing work-management data without approval.
metadata:
  version: 1.6.0
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
- local sprint-goal publication root, per-sprint folder pattern, filename case and note basenames, and whether an external goal tool is a manual publishing destination;
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

Display **all capacity figures as whole numbers, rounded to the nearest integer** (halves round up for these non-negative values). This includes individual and total days/hours, historical medians, percentages, gross estimates, ticket counts and story points in prose, tables and rendered Dataview output. Keep source inputs and intermediate calculations at full precision; round only for presentation. Show capacity factors as whole-number percentages instead of decimal multipliers. Calculate totals from unrounded inputs, not rounded rows. Add a short rounding note where displayed rows do not sum to the rounded total.

`available people-days = sum(each contributor's scheduled working days) − PTO − public holidays − known non-delivery commitments`

Show the team roster/role, absences, individual available days, baseline people-days, lost days, available people-days and availability percentage. Do not count a lead, quality engineer, product partner or shared specialist as full development capacity unless their actual contribution is known; show it separately. Apply a sensible contingency to the delivery commitment (normally around 10%, adjusted for recent unplanned work and delivery risk), and show it explicitly.

Use each team's **six most recent completed sprints** as the forecast baseline; exclude active and future sprints. Record completed stories, tasks, bugs, other ticket types, total tickets and completed story points separately for every sprint, alongside its actual working days, historical roster and calendar-adjusted available people-days. Paginate to completion and count each ticket once per sprint; exclude subtasks and epic wrappers. Use the completed-at-close snapshot when available, otherwise label a resolution-timestamp reconstruction as a proxy. Do not silently omit an unusual sprint. If fewer than six completed sprints exist, show the smaller sample and its limitation.

Forecast with that team's six-sprint trailing medians, adjusted for new availability:

`adjusted point forecast = trailing median completed story points × (new available people-days / historical median available people-days)`

`adjusted throughput forecast = trailing median completed ticket count × (new available people-days / historical median available people-days)`

Show the gross availability-adjusted ticket and point estimates, then apply the explicit operating reserve once and round the planning allowances to the nearest whole ticket/point. Estimate the story/task/bug mix from their shares of completed tickets across the same six sprints; retain an Other category where needed. Allocate the rounded total-ticket allowance across those shares using largest remainders so the categories sum exactly to the total. Label the mix as indicative, not a quota or a conversion between points and tickets. Include planned bugs/maintenance in the baseline; do not charge the same work to the contingency again. Keep unplanned reserve and pull work separate.

Start each team's **Capacity planning** section with **Summary for Atlassian**, a short, self-contained block that can be copied directly into the external goal (aim for no more than 150 words). Include the sprint period; one bullet per absent contributor with rounded days lost and brief dates/reason (or 'No recorded holidays or absences'); delivery people-days after reserve, including separate QE days; recommended stories/tasks/bugs and Other when non-zero; and total tickets and story points. End with one short sentence that these are six-sprint, availability-adjusted estimates subject to carry-over and verification effort. Use plain Markdown bullets and whole numbers; do not put Dataview, code, history tables, formulas, internal wikilinks or detailed caveats inside the copy-ready block. Recommendations must match the supporting calculations and reconcile to the total.

Follow the summary with **Supporting detail**, containing the six-sprint history, capacity adjustment, type mix, total-ticket allowance and point allowance. Verify historical rosters and working patterns rather than copying today's headcount backwards. If history is incomplete, seek confirmation or give a clearly labelled conditional estimate with explicit assumptions and sensitivity; never present assumed historical people-days as measured.

Use the lower credible forecast if a material release dependency, unusually high bug load, team change or migration makes the normal run-rate optimistic. Point and ticket-count forecasts are complementary: do not convert tickets to points and never compare velocity between teams.

## Draft and publish the planning tree

Read the destination directory's `AGENTS.md` before authoring. Group each sprint's parent, team and evidence notes in one sprint-specific subdirectory beneath the configured publication root. Take its folder pattern and document basenames from the Delivery Planning System context; use lowercase hyphen-separated filenames (kebab-case), distinct from human-readable goal titles. Apply this to new sprint notes; move existing notes only when requested. Before moving or renaming notes, inspect inbound links, use the vault's link-aware operation and verify every affected link, including frontmatter. Use sprint-qualified vault-relative wikilinks when repeated basenames could resolve to another sprint. Honour local list-indentation rules in saved Markdown; preserve YAML and code-block syntax.

Where the configured model uses a parent goal and child team goals, finalise the team notes first, then derive the parent sprint summary directly from their current project goals. Do not copy an older parent, introduce additional scope or turn an aim or blocked item into a commitment. Keep the parent brief and outcome-led; detailed capacity, forecasts and planning evidence belong in the team/evidence notes.

For the parent document, use one short paragraph per non-empty team-plan section, introduced by a bold inline label: **Ready to release:**, **In progress:**, **Planning / Upcoming:**, **Discovery:** where active, then the active programme label and **Maintenance:**. Keep each section on its own line using a Markdown hard line break (two trailing spaces or a backslash followed by a single newline), with **no blank lines between sections**. Do not use section headings, bullet lists, a separate Sprint focus section or a capacity report in the parent body. Do not repeat the sprint date range already recorded in metadata; retain meaningful project deadlines and gating dependencies.

Link every referenced project directly to its Jira epic, using the team notes' verified links. Preserve section grouping and each team's prioritised project order; do not reorder by a newly inferred priority. Where two teams' orders differ, use brief team-specific clauses to preserve both rather than implying an agreed cross-team ranking. Summarise shared programme work once while retaining distinct contributions and source order. Keep human-readable titles, external-goal reference and team/evidence links in frontmatter so the body remains copy-ready. Follow the worked shape in [the goal template](references/goal-template.md).

Use the configured live goal format, guided by [the goal template](references/goal-template.md). Each team note is a portfolio view, not a ticket dump: represent every project in its relevant lane as a linked Jira epic or initiative followed by a short, nested objective and named lead. The objective must state the meaningful sprint end state: a percentage/milestone of the epic, release readiness, or verification. Do not invent a completion percentage or confidence label; state delivery risk in the objective or parent dependency summary when the evidence supports it. Write `N/A` when Planning / Upcoming or Discovery has no work. Keep team allocation out of the parent but explicit in the appropriate team note.

For each project entry, read the linked epic's **Portfolio Status** section and use its single named **engineering lead** as the sole value of the `Lead` bullet. Where the section lists engineering, product and design leads, take only the engineering lead. Do not substitute the epic assignee, child-ticket assignee, QE owner, EM, product partner, dependency owner or sprint contributor unless that person is explicitly the engineering lead in Portfolio Status. Write only that person's name after `Lead:`; do not append other names, responsibilities or coordination notes. Put implementation, verification and dependency responsibilities in the objective or dependency summary instead.

If Portfolio Status has no engineering lead, lists multiple engineering leads or conflicts about who holds that role, flag the ownership gap in planning evidence and seek clarification. Do not choose arbitrarily, invent a lead, list co-leads or publish the entry as ready. Keep it `needs-review` and omit the unresolved `Lead` bullet until one engineering lead is confirmed.

An initiative may appear in more than one team note when the teams have distinct, named contributions (for example, a shared release gate or programme milestone). Make each team's objective specific; retain the same engineering lead from the epic's Portfolio Status wherever that epic appears. Team-specific implementers and verification owners belong in the objective or dependency summary. Do not present the same full commitment twice.

When the user asks to publish the plan, create the configured local notes with Obsidian-compatible frontmatter. Add wikilinks between the parent and team notes when they share a directory, and retain normal Markdown links for external systems. Use concise bullets in the copy-ready capacity summary; retain compact capacity/absence tables and Dataview in Supporting detail. Mark the notes as `ready-to-publish` unless a material evidence gap means they need review. Do not make the ability to write an external goal a prerequisite for publishing the local artefact.

Before proposing the plan, run this quality gate:

- Every roadmap release in the sprint window that depends on the teams is represented, explicitly deferred or marked at risk.
- Each must-deliver outcome has an owner, measurable end state, verification/release path and dependency status.
- Every project `Lead` bullet contains exactly one engineering lead verified against that epic's Portfolio Status, with no extra names or role commentary. Missing or multiple engineering leads are flagged for clarification and keep the entry in review.
- Each team’s Capacity planning section starts with a copy-ready Summary for Atlassian covering absences, rounded days, recommended type mix, total tickets and points; its numbers match Supporting detail.
- Each team’s Capacity planning section shows the six completed sprints, forecast stories/tasks/bugs/other, total tickets and story points, adjusted for team availability and the explicit reserve. Type counts sum to total tickets. Committed points/tickets fit those allowances and verification capacity; assumptions and any smaller sample are visible.
- Cross-team initiatives have distinct team contributions; hand-offs have a named receiving owner and date.
- Maintenance, bugs and cross-cutting programme capacity are visible rather than absorbed into an implausible feature commitment.
- The parent is grounded in the current team goals, groups linked epics in their section/priority order, uses compact bold-labelled sections separated by single hard line breaks without blank lines, and omits the repeated sprint period. The team goals retain delivery and capacity detail.
- All sprint notes share the configured sprint folder, filenames use lowercase hyphens, internal and frontmatter links resolve to the intended sprint, and nested lists follow the local indentation convention.

## During and after the sprint

At planning, record baseline commitments separately from anything added later. At sprint end, record completed-at-close measures separately for each team: committed/completed story points, committed/completed ticket count, spillover count and reason, unplanned ticket count, work-type mix, people-days, and whether each must-deliver/release outcome landed. Use the trend to improve the next forecast; never use it to rank individuals or teams.

Call out a meaningful change: a sustained fall in availability-adjusted throughput, repeated spillover at the same stage, a release repeatedly missing readiness work, or a growing unplanned-work share. Investigate the constraint (scope, dependencies, quality, support load or workflow) before changing the velocity target.
