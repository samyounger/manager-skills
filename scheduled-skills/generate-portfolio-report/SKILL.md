---
name: generate-portfolio-report
description: Generates a configured engineering portfolio report when explicitly requested or invoked by an authorised reporting schedule.
metadata:
  version: 1.3.0
---

# Generate a portfolio report

Run only when explicitly invoked or by an authorised scheduler. Reviewing this file is not an instruction to run it. Follow [the configuration contract](../README.md#configuration-contract) before taking action. Read the caller-supplied private configuration, personal preferences and management context. Do not assume an organisation, timezone, language, calendar, output location or scheduler. If a required input is missing, stop and report the missing setting; do not invent it or create a schedule.

## Before acting and after writing

- Confirm required private settings, referenced files and live tool access. Stop and report missing inputs or access.
- Read all required evidence and pagination. Do not publish from failed or incomplete queries.
- Compare proposed content with the current destination. Skip unchanged output; preserve concurrent and human edits using the destination's conditional-update method.
- Read back saved changes and verify the intended content and links. Record material blockers or partial completion privately so a retry can reuse valid output.

## Workflow

1. Read the configured portfolio or delivery team, manager, issue scope, report destination, filename pattern, timezone and writing method. Derive dates using that timezone.
2. Load the reusable `delivery-manager` skill and its references from the configured repository's `skills/delivery-manager/`. Supply the configured caller context and read live evidence through the available issue-tracker connector or CLI.
3. Generate the report using that skill's evidence rules and output format. Compare with the previous report where applicable, preserve historical reports and update only the configured current report.
4. Write using the method authorised for this installation, following the destination's instructions. Destination frontmatter rules take precedence over the reusable report template. Use the one configured output path and writing method. Do not embed a filesystem or Obsidian CLI exception in the shared skill; any explicit caller override belongs in private configuration.
5. Do not change issue-tracker records as part of report generation. Report unavailable evidence or output access clearly.

## Setup

See the sibling `configuration.example.md` for required inputs and [scheduled workflow setup](../../schedules/README.md) for installation. Keep filled-in configuration and schedule snapshots in the repository's ignored `.agents/` directory. Never copy personal inputs or standing authorisation into this shared skill.
