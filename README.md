# Commit Cron

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

A simple GitHub Actions experiment that creates a random number of automated commits every day.

## What it does

Every day at approximately **10:12 AM Asia/Manila time**, the workflow:

1. Selects a random number from **1 through 30**.
2. Updates `bot/last-run.txt` once for each generated commit.
3. Records the workflow run, timestamp, and current commit number.
4. Configures the commit author using the repository owner’s GitHub identity.
5. Creates the selected number of commits.
6. Pushes all generated commits to the current branch.

For example, the workflow may create **5 commits today** and **30 commits tomorrow**.

The workflow can also be started manually from the **Actions** tab.

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

## Random commit count

The workflow uses `shuf` to select a random whole number from **1 through 30**:

```bash 
COMMIT_COUNT="$(shuf -i 1-30 -n 1)"
```

It then runs a loop once for each commit:

```bash 
for i in $(seq 1 "$COMMIT_COUNT"); do
  # Update the generated file
  # Stage the change
  # Create a commit
done
```

The commit number is written to `bot/last-run.txt`, ensuring that the file changes during every loop iteration. 

An example generated file may contain:

```text 
Last automatic update: 2026-07-21 10:12:05 +08:00 (Asia/Manila)
Workflow run: 123456789
Commit: 3 of 5
```

The generated commit messages follow this format:

```text 
chore: daily automated update (1/5)
chore: daily automated update (2/5)
chore: daily automated update (3/5)
chore: daily automated update (4/5)
chore: daily automated update (5/5)
```

All commits are created locally inside the workflow runner and pushed together after the loop finishes.

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

For organization-owned repositories, `github.repository_owner` refers to the organization rather than an individual account. In that case, the commit name and email may need to be configured manually.

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

- Scheduled and manually triggered GitHub Actions workflows
- Randomized automation using shell commands
- Creating multiple commits inside one workflow run
- GitHub Actions context variables - Dynamic commit author configuration
- Automated file updates, commits, and pushes
- Repository write permissions through `GITHUB_TOKEN`
- Fork-friendly workflow configuration

## Important note

The generated commits only confirm that the automation ran successfully. They do not represent manual development activity or meaningful project contributions. 

The number of commits is randomly selected from **1 through 30** during each workflow run. Creating multiple commits does not create multiple GitHub Actions jobs; all commits are generated inside the same job. 

Scheduled GitHub Actions runs may be delayed during periods of high demand. For this reason, `bot/last-run.txt` records the actual execution time rather than the scheduled time.

## License

This project is licensed under the [MIT License](LICENSE).
