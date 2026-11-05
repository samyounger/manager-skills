# Configuration template: weekly-portfolio-snapshot

Copy into `<checkout>/.agents/scheduled-workflows/weekly-portfolio-snapshot.md`. Fill every placeholder there, including explicit page-update authorisation if applicable. The executing agent must stop if required inputs are missing or placeholders remain.

```json
{
  "schema_version": 1,
  "repository": "<absolute-checkout-path>",
  "preferences": "<checkout>/.agents/personal-preferences.md",
  "context": "<checkout>/.agents/em-context.md",
  "timezone": "<IANA-timezone>",
  "language": "<locale>",
  "workflow": "weekly-portfolio-snapshot",
  "required_tools": [
    "jira",
    "confluence-read",
    "confluence-conditional-write"
  ],
  "settings": {
    "page_url": "<page-url>",
    "page_id": "<page-id>",
    "site_url": "<site-url>",
    "release_filters": {
      "<period>": "<verified-filter-id>"
    },
    "release_period_rule": "calendar-quarter",
    "metrics": {
      "unresolved_release": "<verified-query>",
      "active": "<verified-query>",
      "attention": "<verified-query>",
      "upcoming": "<verified-query>",
      "discovery": "<verified-query>",
      "open_bugs_support": "<verified-query>",
      "total_bugs_support": "<verified-query>"
    },
    "update_authorised": false,
    "write_method": "confluence-versioned-rest",
    "release_not_ready": "<release-not-ready>",
    "release_ready": "<release-ready>",
    "reporting_policy": "<reporting-policy>",
    "layout_policy": "<layout-policy>"
  }
}
```
