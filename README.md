# Commit Cron
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

A simple GitHub Actions experiment that creates one automated commit every day.

## What it does

Every day at **10:12 AM Asia/Manila time**, the workflow:

1. Runs automatically through GitHub Actions.
2. Updates `bot/last-run.txt` with the latest execution time.
3. Creates a commit using the `github-actions[bot]` account.
4. Pushes the commit to the repository.

## Try it yourself

You are welcome to fork this project and run the automation in your own GitHub repository.

1. Click **Fork** at the top of this repository.
2. Open the **Actions** tab in your fork.
3. Enable GitHub Actions when GitHub asks for confirmation.
4. Select **Daily Commit Bot**.
5. Click **Run workflow** to test it manually.

After GitHub Actions is enabled, the workflow will also run automatically according to its schedule.

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

This schedule runs once per day at 10:12 AM Philippine time.

## Purpose

This repository is a small demonstration of:

* Scheduled GitHub Actions workflows
* Automated file updates
* Automated Git commits
* Repository write permissions using `GITHUB_TOKEN`

## Important note

The generated commits only confirm that the scheduled automation ran successfully. They do not represent manual development activity or meaningful project contributions.

> [!WARNING]
> Scheduled GitHub Actions are not guaranteed to start at exactly 10:12 AM. Runs may be delayed during periods of high demand, so `bot/last-run.txt` records the actual execution time.

## License

This project is licensed under the [MIT License](LICENSE).
