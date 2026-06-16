# Daily Activity & Learning Log Automation

A lightweight GitHub automation that keeps a consistent learning/activity log
on your repository. Every day, a GitHub Actions workflow updates a timestamp
file and appends a meaningful entry to a learning log, then commits and pushes
the change automatically.

This is built to encourage **coding consistency** and **project documentation**,
not to create empty or meaningless commits.

## What This Automation Does

- Runs automatically once per day using a cron schedule.
- Can also be triggered manually from the GitHub Actions tab.
- Updates `activity.txt` with the current date and time (UTC).
- Appends a new dated entry to `learning-log.md` documenting your daily focus.
- Commits the changes as the official `github-actions[bot]` user.
- Pushes the commit back to the same repository.
- Exits safely (no failure) if there are no changes to commit.

## Repository Structure

```
.
├── README.md                       # This file
├── activity.txt                    # Holds the latest update timestamp
├── learning-log.md                 # Daily learning entries (appended over time)
└── .github/
    └── workflows/
        └── daily-update.yml        # The GitHub Actions workflow
```

## How to Set It Up

1. Create a new repository on GitHub (or use an existing one).
2. Add these files to the repository, keeping the same folder structure:
   - `README.md`
   - `activity.txt`
   - `learning-log.md`
   - `.github/workflows/daily-update.yml`
3. Commit and push them to the default branch (usually `main`).
4. Open the **Actions** tab on GitHub. If prompted, enable workflows for the repo.

That's it. No external accounts, tokens, or paid services are required. The
workflow uses the built-in `GITHUB_TOKEN`, and the `permissions: contents: write`
setting in the workflow allows it to push commits.

> **Tip:** Make sure Actions have write permission. Go to
> **Settings → Actions → General → Workflow permissions** and select
> **"Read and write permissions"** if pushes are blocked.

## How to Manually Run the Workflow

1. Go to the **Actions** tab in your repository.
2. Select **Daily Update** from the list of workflows on the left.
3. Click the **Run workflow** button, choose the branch, and confirm.

This is handy for testing the workflow without waiting for the scheduled time.

## How to Change the Cron Timing

The schedule lives in `.github/workflows/daily-update.yml`:

```yaml
schedule:
  - cron: "30 1 * * *"
```

The cron format is:

```
┌───────────── minute (0 - 59)
│ ┌───────────── hour (0 - 23)
│ │ ┌───────────── day of the month (1 - 31)
│ │ │ ┌───────────── month (1 - 12)
│ │ │ │ ┌───────────── day of the week (0 - 6, Sunday = 0)
│ │ │ │ │
* * * * *
```

Examples:

| Goal                         | Cron expression   |
| ---------------------------- | ----------------- |
| Every day at 01:30 UTC       | `30 1 * * *`      |
| Every day at midnight UTC    | `0 0 * * *`       |
| Every day at 18:00 UTC       | `0 18 * * *`      |
| Every Monday at 09:00 UTC    | `0 9 * * 1`       |

**Important:** GitHub Actions cron times are always in **UTC**, not your local
time zone. Convert your desired local time to UTC before setting the schedule.
You can use a tool like [crontab.guru](https://crontab.guru) to build and verify
expressions.

> Note: Scheduled workflows can sometimes start a few minutes late when GitHub's
> runners are busy. This is normal.

## Notes About the GitHub Contribution Graph

- Commits made by this workflow are authored by `github-actions[bot]`, the
  official GitHub Actions identity.
- Bot commits **do not** count toward your personal contribution graph (the
  green squares on your profile). The contribution graph only counts commits
  associated with **your** GitHub account's verified email.
- The real value here is maintaining a genuine, timestamped learning log and a
  consistent automation habit, not gaming the contribution graph.
- If you want commits to count toward your graph, you would need to author them
  with your own account's email and a personal access token. That is beyond the
  scope of this beginner-friendly setup and is intentionally avoided to keep the
  log honest and the configuration simple.

## License

Use this freely for your own learning and projects.
