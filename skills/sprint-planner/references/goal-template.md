# Published parent and team goal-note template

Read the latest configured sprint goals before drafting. Their live structure is the primary style reference; this is a reusable shape, not a frozen copy of one organisation's content. Publish the result as Markdown notes that can be pasted into the configured goal-sharing tool.

## Titles, frontmatter and links

- Take the parent and child naming pattern from `.agents/em-context.md`.
- Create a parent note and one note per team. Link them with Obsidian wikilinks when published in the same vault.
- Give every note concise frontmatter: `title`, `aliases` where useful, `sprint`, `period`, `type`, `status: ready-to-publish` and relevant tags. Team notes also name `team` and `parent`.
- Derive start and end dates from the configured sprint-boundary rule; verify the prior sprint's actual completion date.

## Parent note

Use this compact, outcome-led structure. The opening paragraph is a synthesis, not a list of child-note tasks.

```text
# <Product> sprint <number>

During <sprint>, <product> will <combined release or customer outcome>, <second verified outcome>, and <material programme outcome>. Alongside these priorities, the teams will <important active work>.

## Sprint focus

- **Release readiness:** <release gates, verification and intended release date>.
- **Product delivery:** <the important team outcomes>.
- **Programme or migration progress:** <the next verified milestones>.
- **Discovery and operational health:** <discovery themes and protected production/maintenance capacity>.

## Capacity and dependencies

The teams have **<available people-days> engineering people-days** available across the sprint after <PTO days> days of IC PTO. Each team reserves <contingency>% for non-feature delivery, so <priority trade-off>.

Key dependencies are <dependency and owner or receiving team>, <verification/release gate>, and <other material dependency>. <Release> has a <target date>; this sprint's outcome is <readiness or launch>.

## Team goals

- [[<team note>|<team> - Sprint <number>]]
```

Omit a focus bullet only when it has no material content. Do not introduce a warning callout merely because a source was unavailable during planning; use one only for an unresolved issue that changes the planned commitment.

## Team note

Keep project work concise and outcome-based. Use linked Jira epics or initiatives followed by a nested objective and named lead. The objective states the meaningful sprint end state: a measured portion/milestone, release readiness or verification. Do not invent a percentage or confidence label.

```text
# <Team> - Sprint <number>

Parent: [[<parent note>|<Product> sprint <number>]]

## Features

### Releasing
- <linked release/initiative>:
  - Objective: <release-ready or launch outcome>, by <date where known>.
  - Lead: <name>.

### In progress
- <linked initiative>:
  - Objective: <named milestone, completion or verification outcome>.
  - Lead: <name>.

### Planning / Upcoming
N/A

### Discovery
In no particular priority order:
- <linked opportunity or initiative>

## <Active cross-cutting programme>

Programme board: <inline link>.

- <workstream>:
  - <linked milestone>
    - Objective: <next verified outcome>.
    - Lead: <name>.

## Maintenance

Reserved contingency for production issues, defects and urgent operational work.

- <linked bugs holding epic>
  - Objective: <what is captured and how it will be handled>.
  - Lead: <name>.
- <linked maintenance holding epic>
  - Objective: <what is captured and how it will be handled>.
  - Lead: <name>.

## Capacity planning

<team calendar link>.

<Dataview capacity block, or a compact table when Dataview is unavailable>
```

### Formatting conventions

- Preserve the configured live section names. The `Features` lanes—**Releasing**, **In progress**, **Planning / Upcoming** and **Discovery**—are the default.
- Use the relevant label consistently: `Lead` for team-owned delivery; `Project lead` only where that is the organisation's existing convention.
- An initiative can appear in multiple team notes only where each team has a distinct objective and named lead. Do not duplicate the full programme commitment.
- Include a cross-cutting programme section only while it is active for that team. Preserve its board link and use nested bullets for workstream ownership.
- Include `Maintenance` whenever bugs or maintenance are planned. Verify holding links before copying them.
- Use real release dates, not “this sprint”. Name an absent lead as a risk; do not silently write `Unassigned` as a normal plan state.
- When **Planning / Upcoming** or **Discovery** has no work, write `N/A`; do not add explanatory prose or a placeholder project.
- Use a Dataview block for repeatable in-vault capacity calculations when the vault supports it. Otherwise use a compact Markdown table. In both forms, distinguish IC capacity from team-lead/partner PTO and show the protected contingency.
