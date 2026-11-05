# Configuration template: prepare-1on1-agendas

Copy into `<checkout>/.agents/scheduled-workflows/prepare-1on1-agendas.md`. Fill every placeholder there, including explicit page-update authorisation if applicable. The executing agent must stop if required inputs are missing or placeholders remain.

```json
{
  "schema_version": 1,
  "repository": "<absolute-checkout-path>",
  "preferences": "<checkout>/.agents/personal-preferences.md",
  "context": "<checkout>/.agents/em-context.md",
  "timezone": "<IANA-timezone>",
  "language": "<locale>",
  "workflow": "prepare-1on1-agendas",
  "required_tools": [
    "calendar",
    "obsidian"
  ],
  "settings": {
    "calendar_id": "<calendar-id>",
    "lookahead_hours": 12,
    "vault": "<vault>",
    "agenda_root": "<agenda-root>",
    "weekly_root": "<weekly-root>",
    "template": "<template>",
    "write_method": "obsidian-cli",
    "cli_path": "<cli-path>",
    "vault_name": "<vault-name>",
    "agenda_filename": "{date}--{person_id}-1-1.md",
    "person_id_rule": "<person-id-rule>",
    "weekly_link_label": "<weekly-link-label>",
    "jira_base_url": "<jira-base-url>",
    "direct_reports": [
      "<confirmed-direct-report>"
    ]
  }
}
```
