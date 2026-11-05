# Reusable scheduled skills

These skills define reusable procedures. They contain no installation's personal inputs or live schedule settings.

| Skill | Purpose |
| --- | --- |
| [Prepare 1:1 agendas](prepare-1on1-agendas/SKILL.md) | Prepare agendas for configured direct reports and optionally link them in weekly notes |
| [Generate a portfolio report](generate-portfolio-report/SKILL.md) | Run the reporting skill with configured team scope and output |
| [Refresh a portfolio snapshot](weekly-portfolio-snapshot/SKILL.md) | Update configured page metrics from verified live queries |

## Configuration contract

Configuration is a private Markdown file containing one fenced JSON object. Copy the selected skill's JSON example, fill in its inputs, and have the executing agent check required settings and access before acting. The JSON block is authoritative. The caller supplies an explicit configuration file path, normally `<repository>/.agents/scheduled-workflows/<skill-name>.md`. Resolve the repository from that path or the canonical symlink target, not from the unrelated project where the scheduled task runs. If no path was supplied, inspect the canonical skill's repository `.agents/scheduled-workflows/<skill-name>.md`. If absent, request the missing configuration without taking external action.

Resolve all `.agents/` lookups in this workflow and its referenced reusable skills against the configured repository, including `em-context.md`, `reports/` and personal preferences. Read `.agents/personal-preferences.md` when present and `.agents/em-context.md` for management context. Resolve configured relative paths against the configuration file's directory. Do not invent missing required context. Only ask for fields required by the selected workflow. Refer to sibling configuration examples for input requirements.

Private configuration supplies identities, team scope, destinations, tool choices, language, timezone and explicit authorisations. It does not grant credentials or override platform access checks. Reusable procedures and neutral examples belong in tracked files. User data, real service URLs and identifiers, machine paths, schedule state and personal exceptions belong only under ignored `.agents/`.

See [setup and recovery](../schedules/README.md). Editing or installing a skill never creates or enables a schedule.

## Supported integrations and execution checks

The current workflows use Jira, Obsidian CLI for agendas/reports, and Confluence for snapshots. Confirm the configured templates and integration access during setup. Other trackers or note stores require adapting the workflow instructions.

The agent executes each skill directly using its available tools. Confirm required inputs, read all source pages, preserve concurrent edits, skip unchanged output and read back saved changes. A configured path or tool name does not prove live access. Keep any personal verification evidence under `.agents/runs/`.
