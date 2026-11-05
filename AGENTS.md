# AGENTS.md

Guidelines for AI agents working in this repository.

## Repository Overview

This repository contains **Agent Skills** for engineering managers, following the [Agent Skills specification](https://agentskills.io/specification.md). Skills install to `.agents/skills/` (the cross-agent standard).

- **Name**: Engineering Management Skills
- **License**: MIT

## Repository Structure

```
engineeringmanagementskills/
├── skills/                # Agent Skills
│   └── skill-name/
│       ├── SKILL.md       # Required skill file
│       └── references/    # Optional supporting docs
├── CONTRIBUTING.md
├── AGENTS.md
├── LICENSE
└── README.md
```

## Skill File Requirements

Every `SKILL.md` must have valid YAML frontmatter:

```yaml
---
name: skill-name
description: When to use this skill. Include trigger phrases.
metadata:
  version: 1.0.0
---
```

Rules:
- `name` must match directory name exactly
- `name` is 1-64 chars, lowercase alphanumeric + hyphens only
- `description` is 1-1024 characters
- Keep `SKILL.md` under 500 lines; move reference material to `references/`

## Foundation Skill

`em-context` is the foundation. All other skills should instruct the agent to read `.agents/em-context.md` (or `.agents/skills/em-context/SKILL.md`) before proceeding, so they have team and org context.

## Reusable workflows and private configuration

Both `skills/<name>/SKILL.md` and `scheduled-skills/<name>/SKILL.md` are reusable, tracked skills. Keep every user's personal and organisation-specific data in this repository's ignored `.agents/` directory. This includes identities, team scope, service URLs and IDs, real machine paths, timezone/language preferences, schedule exports, notification settings and standing authorisation. Never embed these values in shared procedures or examples.

Use `.agents/scheduled-workflows/<skill-name>.md` for filled-in workflow inputs and `.agents/schedules/<app>/` for recovery settings. Read `.agents/personal-preferences.md` when present and `.agents/em-context.md` before management work. The caller supplies the configuration path; resolve it independently of the current working project. Request missing required inputs without inventing defaults or taking external action.

Follow [scheduled workflow setup](schedules/README.md) when setting up this repository or migrating schedules. It defines installation, configuration, backup, cross-app ownership and verification rules. Verify `.agents/` is ignored and has no tracked files; ignore rules alone do not protect already staged content. Keep runtime scheduler state app-managed and one active scheduler per workflow.

## Scheduled workflow execution

Read the authoritative private configuration and confirm required settings, tools and destination access before acting. Follow the selected skill's checks for complete evidence, conflict-safe updates, repeat-run behaviour and saved-result verification. Report missing inputs or access clearly. Keep schedule settings unchanged unless the user requests a scheduling change.
