---
name: weekly-portfolio-snapshot
description: Refreshes a configured portfolio snapshot when explicitly requested or invoked by an authorised recurring schedule.
metadata:
  version: 1.3.0
---

# Refresh a portfolio snapshot

Run only when explicitly invoked or by an authorised scheduler. Reviewing this file is not an instruction to run it. Follow [the configuration contract](../README.md#configuration-contract) before taking action. Read the caller-supplied private configuration, personal preferences and management context. Do not assume an organisation, timezone, language, calendar, output location or scheduler. If a required input is missing, stop and report the missing setting; do not invent it or create a schedule.

## Before acting and after writing

- Confirm required private settings, referenced files and live tool access. Stop and report missing inputs or access.
- Read all required evidence and pagination. Do not publish from failed or incomplete queries.
- Compare proposed content with the current destination. Skip unchanged output; preserve concurrent and human edits using the destination's conditional-update method.
- Read back saved changes and verify the intended content and links. Record material blockers or partial completion privately so a retry can reuse valid output.

## Workflow

1. Read the configured destination page, issue-tracker filters or queries, metric definitions, release-period mapping, layout constraints and authorised writing method. A configuration must identify the target page and explicitly permit the intended update.
2. Determine the current release period using the configured timezone and period rules. Resolve an existing matching release filter. Never guess a filter ID; if the mapping is missing, preserve the release label and request the missing input.
3. Read live counts for the configured release, active work, attention items, upcoming work, discovery and unresolved bugs or support issues. Apply the configured status and scope rules; do not turn a failed query into a zero count. Retain total bug volume if requested and available.
4. Read the current page before editing. Preserve its intentional layout and source links, changing only the configured date, release label/link, figures and conclusion. Follow the caller's content-format and concise-layout requirements.
5. Apply the configured release-readiness rule and allowed wording. A zero unresolved count may support a recommendation, but does not authorise closing a release.
6. Update only the configured page through a version-conditional REST write, after every configured metric query completed. Stop without publishing if conditional-write access is unavailable. Do not change issue records or broader permissions. Report changes or blockers according to the caller's preferences; scheduler notification settings remain app-managed.

## Setup

See the sibling `configuration.example.md` for required inputs and [scheduled workflow setup](../../schedules/README.md) for installation. Keep filled-in configuration and schedule snapshots in the repository's ignored `.agents/` directory. Never copy personal inputs or standing authorisation into this shared skill.
