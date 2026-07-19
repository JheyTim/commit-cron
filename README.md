# Commit Cron

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

A simple GitHub Actions experiment that creates one automated commit every day.

## What it does

Every day at approximately **10:12 AM Asia/Manila time**, the workflow:

1. Updates `bot/last-run.txt` with the actual execution time.
2. Configures the commit author using the repository owner’s GitHub identity.
3. Creates a commit containing the updated file.
4. Pushes the commit to the current branch.

The workflow can also be started manually from the Actions tab.

## Schedule

```yaml
schedule:
  - cron: "12 10 * * *"
    timezone: "Asia/Manila"
```

Manual runs are enabled through:

```yaml
workflow_dispatch:
```

## Commit identity

For repositories owned by personal accounts, the workflow builds the commit identity from these GitHub Actions context values:

```yaml
env:
  COMMIT_NAME: ${{ github.repository_owner }}
  COMMIT_EMAIL: "${{ github.repository_owner_id }}+${{ github.repository_owner }}@users.noreply.github.com"
```

It then configures Git before creating the commit:

```bash
git config user.name "$COMMIT_NAME"
git config user.email "$COMMIT_EMAIL"
```

The generated email follows GitHub’s private email format:

```text
ACCOUNT_ID+USERNAME@users.noreply.github.com
```

This avoids exposing a personal email address and allows the workflow to adapt automatically when the repository is forked.

## Try it yourself

1. Fork this repository.
2. Open the **Actions** tab in your fork.
3. Enable GitHub Actions when prompted.
4. Select **Daily Commit Bot**.
5. Click **Run workflow** to test it.

After Actions is enabled, the workflow will also run according to its daily schedule.

## Repository structure

```text
.
├── .github/
│   └── workflows/
│       └── daily-commit.yml
├── bot/
│   └── last-run.txt
└── README.md
```

## Purpose

This repository demonstrates:

* Scheduled and manually triggered GitHub Actions workflows
* GitHub Actions context variables
* Dynamic commit author configuration
* Automated file updates, commits, and pushes
* Repository write permissions through `GITHUB_TOKEN`
* Fork-friendly workflow configuration

## Important note

The generated commits only confirm that the automation ran successfully. They do not represent manual development activity or meaningful project contributions.

Scheduled GitHub Actions runs may be delayed during periods of high demand. For this reason, `bot/last-run.txt` records the actual execution time rather than the scheduled time.

## License

This project is licensed under the [MIT License](LICENSE).
