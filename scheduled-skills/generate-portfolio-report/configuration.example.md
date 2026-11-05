# Configuration template: generate-portfolio-report

Copy into `<checkout>/.agents/scheduled-workflows/generate-portfolio-report.md`. Fill every placeholder there, including explicit page-update authorisation if applicable. The executing agent must stop if required inputs are missing or placeholders remain.

```json
{
  "schema_version": 1,
  "repository": "<absolute-checkout-path>",
  "preferences": "<checkout>/.agents/personal-preferences.md",
  "context": "<checkout>/.agents/em-context.md",
  "timezone": "<IANA-timezone>",
  "language": "<locale>",
  "workflow": "generate-portfolio-report",
  "required_tools": [
    "jira",
    "obsidian"
  ],
  "settings": {
    "portfolio": "<portfolio>",
    "manager": "<manager>",
    "jira_base_url": "<jira-base-url>",
    "jql": "<jql>",
    "output_directory": "<output-directory>",
    "filename_pattern": "{date}--portfolio-report.md",
    "vault": "<vault>",
    "vault_name": "<vault-name>",
    "write_method": "obsidian-cli",
    "cli_path": "<cli-path>"
  }
}
```
