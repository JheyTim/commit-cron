# Commit Cron

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

A simple GitHub Actions experiment that creates one automated commit every day.

## What it does

Every day at **10:12 AM Asia/Manila time**, the workflow:

1. Runs automatically through GitHub Actions.
2. Updates `bot/last-run.txt` with the actual execution time.
3. Configures the commit author using the repository owner’s GitHub username and GitHub-provided `noreply` email address.
4. Creates a commit containing the updated file.
5. Pushes the commit to the repository’s current branch.

For repositories owned by personal accounts, the commit identity is generated from these GitHub Actions context values:

```yaml
github.repository_owner
github.repository_owner_id
```

The resulting email follows GitHub’s private email format:

```text
ACCOUNT_ID+USERNAME@users.noreply.github.com
```

No personal email address is exposed.

## Try it yourself

You are welcome to fork this project and run the automation in your own GitHub repository.

1. Click **Fork** at the top of this repository.
2. Open the **Actions** tab in your fork.
3. Enable GitHub Actions when GitHub asks for confirmation.
4. Select **Daily Commit Bot**.
5. Click **Run workflow** to test it manually.

After GitHub Actions is enabled, the workflow will also run automatically according to its schedule.

In a personal fork, future automated commits will use the fork owner’s GitHub username and GitHub-provided `noreply` email address.

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

## Schedule

```yaml
schedule:
  - cron: "12 10 * * *"
    timezone: "Asia/Manila"
```

This schedule runs once per day at approximately **10:12 AM Philippine time**.

The workflow can also be started manually through the Actions tab using `workflow_dispatch`.

## Commit identity

The workflow dynamically configures the commit author:

```yaml
env:
  COMMIT_NAME: ${{ github.repository_owner }}
  COMMIT_EMAIL: "${{ github.repository_owner_id }}+${{ github.repository_owner }}@users.noreply.github.com"
```

It then applies those values before creating the commit:

```bash
git config user.name "$COMMIT_NAME"
git config user.email "$COMMIT_EMAIL"
```

This makes the workflow reusable across personal forks without hard-coding the original repository owner’s username or email address.

## Purpose

This repository is a small demonstration of:

- Scheduled GitHub Actions workflows
- GitHub Actions context variables
- Dynamic commit author configuration
- Automated file updates
- Automated Git commits
- Repository write permissions using `GITHUB_TOKEN`
- Fork-friendly workflow configuration

## Important notes

The generated commits confirm that the scheduled automation ran successfully. They do not represent manual development activity or meaningful project contributions.

> [!WARNING]
> Scheduled GitHub Actions are not guaranteed to start at exactly 10:12 AM. Runs may be delayed during periods of high demand, so `bot/last-run.txt` records the actual execution time.

## License

This project is licensed under the [MIT License](LICENSE).
