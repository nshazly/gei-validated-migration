# Chat GPT Generated GitHub Enterprise Importer (GEI) Migration Workflow

> 🧠 **Prompt Engineering Note:**  
> This workflow was created through iterative prompt engineering using ChatGPT. The full list of prompts and guidance on how to use them can be found in [docs/prompt_engineering.md](docs/prompt_engineering.md).

This GitHub Actions workflow performs **parallel repository migrations** from **GitHub Enterprise Server (GHES)** to **GitHub Enterprise Cloud (GHEC)** using the [GitHub Enterprise Importer CLI extension](https://github.com/github/gh-gei).

This GitHub Actions workflow performs **parallel repository migrations** from **GitHub Enterprise Server (GHES)** to **GitHub Enterprise Cloud (GHEC)** using the [GitHub Enterprise Importer CLI extension](https://github.com/github/gh-gei).

It supports:
- ✅ Dry-run/audit validation by providing a step to generate repository metadata in json format
- 🚀 Real migration with `gh gei migrate-repo`
- ⚙️ Configurable concurrency
- 📢 Slack and Microsoft Teams notifications (individual + summary)
- 📄 Post-run success/failure reporting

---

## 📁 File: `.github/workflows/gei-validated-migration.yaml`

### 🚀 Workflow Triggers

- `workflow_dispatch` (manual trigger via GitHub UI or API)

---

## 🔧 Inputs

| Input Name     | Required | Description |
|----------------|----------|-------------|
| `repo_list_path` | ✅ | Path to JSON file (e.g. `data/repos.json`) containing a flat array of repo names |
| `source_org`     | ✅ | Source GitHub Enterprise Server (GHES) org name |
| `target_org`     | ✅ | Target GitHub Enterprise Cloud (GHEC) org name |
| `max_parallel`   | ❌ | Maximum number of concurrent migrations (default: `4`) |
| `dry_run`        | ❌ | If `true`, runs the audit step instead of real migration (default: `false`) |

---

## 🧪 Sample `repos.json`

```json
["repo-one", "repo-two", "repo-three"]
```

---

## 🔐 Required Secrets

| Secret Name          | Description |
|----------------------|-------------|
| `GH_PAT_SOURCE`      | PAT with access to GHES repos |
| `GH_PAT_TARGET`      | PAT with admin access to GHEC org |
| `SLACK_WEBHOOK_URL`  | Slack Incoming Webhook URL |
| `TEAMS_WEBHOOK_URL`  | Microsoft Teams Webhook URL |
| `SLACK_THREAD_TS`    | *(Optional)* Slack thread timestamp for threading notifications |

---

## 🧭 Workflow Logic

1. **Matrix Generation**
   - Parses `repos.json` to build a dynamic matrix of repositories.

2. **Dry-Run Audit (if `dry_run: true`)**
   - Runs the audit step in parallel for each repo.
   - Generates `audit-summary.txt` artifact with results.
   - Sends final success/failure count to Slack & Teams.

3. **Migration Execution (if `dry_run: false`)**
   - Runs `gh gei migrate-repo` in parallel for each repo.
   - Generates `migration-summary.txt` artifact with results.
   - Sends per-repo and summary notifications to Slack & Teams.

4. **Slack and Teams Notifications**
   - Individual repo status notifications.
   - Final summary (total succeeded/failed).
   - Slack threading supported via `SLACK_THREAD_TS`.

---

## 📦 Output Artifacts

| File | Description |
|------|-------------|
| `audit-summary.txt` | One line per repo showing audit status |
| `migration-summary.txt` | One line per repo showing migration result |

---

## 📢 Example Final Summary Notification

> **Migration Complete ✅**  
> Total: 20  
> Succeeded: 18  
> Failed: 2

---

## 📘 References

- [GitHub Enterprise Importer Docs](https://docs.github.com/en/migrations/using-github-enterprise-importer)
- [`gh gei` Extension](https://github.com/github/gh-gei)
- [Slack Webhook Setup](https://api.slack.com/messaging/webhooks)
- [Microsoft Teams Webhook Setup](https://learn.microsoft.com/en-us/microsoftteams/platform/webhooks-and-connectors/how-to/add-incoming-webhook)

---

## 🧩 Enhancement Ideas

- Aggregate all results into `summary.csv`
- Automatically open GitHub Issues for failed repos
- Generate formatted HTML or Markdown report
# gei-validated-migration
