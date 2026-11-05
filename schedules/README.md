# Scheduled workflow setup

Keep shared procedures in `skills/` and `scheduled-skills/`. Keep all filled-in personal configuration, automation exports and machine-specific setup under the ignored repository `.agents/` directory. Runtime scheduler storage remains app-managed.

## Set up a checkout

1. Choose the checkout location. Read `AGENTS.md` and the [configuration contract](../scheduled-skills/README.md#configuration-contract).
2. Confirm `.agents/` is ignored and untracked before adding private information. Create `.agents/personal-preferences.md` and `.agents/em-context.md` using the `em-context` skill as appropriate. Keep report profiles and other personal context there too. Never create a second conflicting context store.
3. Copy the selected skill's `configuration.example.md` into `.agents/scheduled-workflows/<skill-name>.md`. Fill the single JSON block with the user's verified context, timezone, destinations, writing method and permissions. Check required settings and configured tool access before execution. Do not fill examples in place. Do not assume access or copy another user's authorisation.
4. Install each selected skill with an individual symlink from the agent's user skill directory to the canonical `scheduled-skills/<skill-name>` directory. Codex uses `~/.agents/skills/`; Claude Code uses `~/.claude/skills/`. Resolve the actual checkout path and preserve unrelated existing links. Install referenced reusable skills as well.
5. Choose one active scheduler per workflow. Its prompt should read the absolute private configuration path afresh on every run, then execute the reusable skill specified there. Keep cadence, model, notification settings, target project/chat and working folders in the scheduler. Save recovery snapshots under `.agents/schedules/<app>/`.
6. Inspect configuration and available tool access first. Run a deliberate workflow test only with authorisation for its actual writes. Configure the schedule through the app's supported automation interface and verify all settings before enabling it. If an update tool normalises legacy settings, compare with a private backup and restore unrelated values.

## Agent rules

- Read the user's private preferences and management context before configuring a workflow. Store new personal or organisation-specific information under `.agents/`, never in shared skills, examples, README files or contributor guidance.
- Keep names, account IDs, service URLs, query/filter/page IDs, real local paths, timezone/language preferences, notification preferences, schedule IDs, chat targets and standing authorisation out of tracked workflow files.
- Do not stage, commit or publish `.agents/`. Git ignore rules do not remove files already in the index: check both ignored status and tracked paths. Move any private staged files out of the index as part of a requested privacy migration, preserving their local contents.
- Back up original definitions under `.agents/` before migration. Preserve active status, cadence, models, notification policy, working folders and targets unless the user requested changes. Verify saved source and symlink targets, not only rendered output.
- Keep one canonical skill copy. Use separate private configurations for multiple installations. Do not rewrite shared skills with values discovered during a run.

## Cross-app use and recovery

Codex and Claude Code can share the canonical skill and private input files. Each app needs its own available tools, permissions and scheduler configuration. Schedule exports are recovery records, not interchangeable live state. Do not symlink runtime scheduler stores or enable duplicate schedules.

To transfer ownership, pause the existing schedule, configure and verify the replacement, then enable the replacement deliberately. An existing-chat target may not have a direct equivalent in another app. Restore a checkout and its private `.agents/` backup separately, recreate skill symlinks, verify machine paths and targets, and keep restored schedules paused until checked. The repository alone does not back up ignored personal data.

## Execution checks

The agent follows the chosen skill directly. Confirm supported integration access, configured templates and an already-open Obsidian vault where required. Read every required source completely, preserve concurrent destination edits, skip unchanged output, and read back saved changes. Test a newly configured workflow with authorised live reads/writes before relying on its schedule. Keep personal verification evidence under `.agents/runs/`.

Back up private context and run settings to a user-chosen private backup destination; Git does not provide this backup. Scheduler prompts must read the current configuration and skill on every run.
