# Published parent and team goal-note template

Read the latest configured sprint goals before drafting. Their live structure is the primary style reference; this is a reusable shape, not a frozen copy of one organisation's content. Publish the result as Markdown notes that can be pasted into the configured goal-sharing tool.

## Titles and dates

- Take the parent and child naming pattern from `.agents/em-context.md`.
- Create a parent note and one note per team. Link them with Obsidian wikilinks when published in the same vault.
- Add concise frontmatter: title, sprint, period, type (`parent` or `team`), status and tags. Keep the body portable so it remains readable outside Obsidian.
- Derive start and end dates from the configured sprint-boundary rule; verify the prior sprint's actual completion date.

## Parent description

Write one short paragraph in natural language. It should combine the most important customer/release outcomes across teams, state the relevant readiness or operational outcome, and disclose a material capacity/dependency risk. Avoid ticket-level assignment and repeated child content.

Example shape:

> During this sprint, the teams will make `<release/customer outcome>` ready for release and complete `<second outcome>`, while reducing `<material risk>`. The plan is based on `<capacity qualification>`; `<dependency or decision>` remains the main delivery risk.

## Team goal description

Use the configured structured format. Keep project work concise and outcome-based. Reference each project once using its linked Jira epic or initiative. Use the nested entry shape below rather than a task list. The objective must say whether the sprint aims to complete a measured portion/milestone of the epic, get ready to release, or complete verification. Do not make up a percentage when the evidence does not provide one.

```text
## Features

### Releasing
- <linked release/initiative>:
  - Objective: get ready to release by completing <release gate or outcome>.
  - Confidence: <high / medium / low>.
  - Project lead: <name or Unassigned>.

### In progress
- <linked initiative>:
  - Objective: complete <percentage or named epic milestone>.
  - Confidence: <high / medium / low>.
  - Project lead: <name or Unassigned>.

### Planning / Upcoming
- <linked initiative>:
  - Objective: prepare <a start-ready scope, decision or planning artefact>.
  - Confidence: <high / medium / low>.
  - Project lead: <name or Unassigned>.

### Discovery
- <linked opportunity/initiative>:
  - Objective: decide or produce <specific artefact>.
  - Confidence: <high / medium / low>.
  - Project lead: <name or Unassigned>.

## Cross-cutting programme
Programme board: <inline link>

- <workstream/role> (<owner>)
  - <linked ticket or milestone> — <next verified outcome>

## Maintenance

- Bugs: <linked holding epic/initiative> (<owner>).
- Maintenance: <linked holding epic/initiative> (<owner>).

## Capacity planning

PTO and public holidays: <team calendar link>.

| Capacity measure | Plan |
|---|---|
| Contributors | <number and roles; identify non-full-time/shared roles> |
| Working days and sprint period | <working days>; <start> to <end> |
| Baseline | <working days> × <full-time-equivalent contributors> = <baseline people-days> |
| Lost capacity | <days> / <baseline people-days> = <percentage>% |
| Available | <available people-days> people-days (<availability>%) |
| Delivery contingency | <people-days or percentage>, for <reason> |
| Commitable capacity | <available minus contingency> people-days |
| Forecast | <point range> story points and <ticket-count range> completed tickets, based on <sprints used> |

| Person | Absence or non-delivery time | Lost delivery capacity |
|---|---|---|
| <person> | <reason and dates> | <days> |

Use one `None recorded` row when there are no planned absences.
```

### Formatting conventions

- Preserve the configured live section names. The `Features` pattern above—**Releasing**, **In progress**, **Planning / Upcoming** and **Discovery**—is a useful default, not a requirement.
- Include a cross-cutting programme section only while it is active for that team. Preserve its board link and use nested bullets for workstream ownership.
- Include `Maintenance` whenever bugs or maintenance are planned. Verify holding links before copying them.
- Use real release dates, not “this sprint”. Use `Project lead: Unassigned` as a planning risk when there is no named lead.
- When **Planning / Upcoming** or **Discovery** has no work, write `N/A`; do not add explanatory prose or a placeholder project.
- Nominal headcount and PTO percentage provide context but are not a forecast. Include actual available people-days and a visible contingency.
