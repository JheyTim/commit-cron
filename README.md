# Commit Cron
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

A simple GitHub Actions experiment that creates one automated commit every day.

## What it does

Every day at **10:00 AM Asia/Manila time**, the workflow:

1. Runs automatically through GitHub Actions.
2. Updates `bot/last-run.txt` with the latest execution time.
3. Creates a commit using the `github-actions[bot]` account.
4. Pushes the commit to the repository.

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

## Run it manually

The workflow can also be started manually:

1. Open the **Actions** tab.
2. Select **Daily Commit Bot**.
3. Click **Run workflow**.
4. Select the branch and confirm.

## Schedule

```yaml
schedule:
  - cron: "0 10 * * *"
    timezone: "Asia/Manila"
```

This schedule runs once per day at 10:00 AM Philippine time.

## Purpose

This repository is a small demonstration of:

* Scheduled GitHub Actions workflows
* Automated file updates
* Automated Git commits
* Repository write permissions using `GITHUB_TOKEN`

## Important note

The generated commits only confirm that the scheduled automation ran successfully. They do not represent manual development activity or meaningful project contributions.

> [!WARNING]
> Scheduled GitHub Actions are not guaranteed to start at exactly 10:00 AM. Runs may be delayed during periods of high demand, so `bot/last-run.txt` records the actual execution time.

## License

This project is licensed under the [MIT License](LICENSE).
