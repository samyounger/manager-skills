---
name: prepare-1on1-agendas
description: Prepares upcoming direct-report 1:1 agendas when explicitly requested or invoked by an authorised agenda schedule.
metadata:
  version: 1.3.0
---

# Prepare upcoming 1:1 agendas

Run only when explicitly invoked or by an authorised scheduler. Reviewing this file is not an instruction to run it. Follow [the configuration contract](../README.md#configuration-contract) before taking action. Read the caller-supplied private configuration, personal preferences and management context. Do not assume an organisation, timezone, language, calendar, output location or scheduler. If a required input is missing, stop and report the missing setting; do not invent it or create a schedule.

## Before acting and after writing

- Confirm required private settings, referenced files and live tool access. Stop and report missing inputs or access.
- Read all required evidence and pagination. Do not publish from failed or incomplete queries.
- Compare proposed content with the current destination. Skip unchanged output; preserve concurrent and human edits using the destination's conditional-update method.
- Read back saved changes and verify the intended content and links. Record material blockers or partial completion privately so a retry can reuse valid output.

## Workflow

1. Read the configured calendar source, look-ahead window, management context and destination for meeting agendas. Resolve direct reports from that context; do not infer reporting relationships from a calendar title.
2. Load the reusable `1on1s` skill from the configured repository's `skills/1on1s/SKILL.md`. Follow its evidence and person-profile rules for each qualifying meeting. Use the configured template, agenda root and filename convention. Jira, GitHub and Slack evidence are prerequisites when a meeting requires them; record missing coverage explicitly and do not invent metrics.
3. Reuse an existing agenda for the same meeting where available. If weekly-note linking is configured, read the relevant weekly note and add the actual agenda link under the correct day without duplicating it. Use the configured vault's link and indentation conventions.
4. Follow the destination's writing instructions and the configured access method. Report missing calendar or vault access without fabricating meetings or notes.
5. Summarise only the created or updated agendas and material blockers, following the caller's reporting preferences. Do not send meeting invitations or messages unless separately authorised.

## Setup

See the sibling `configuration.example.md` for required inputs and [scheduled workflow setup](../../schedules/README.md) for installation. Keep filled-in configuration and schedule snapshots in the repository's ignored `.agents/` directory. Never copy personal inputs or standing authorisation into this shared skill.
