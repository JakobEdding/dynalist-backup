# Dynalist Backup

A GitHub Action to automatically backup the contents of your Dynalist to a GitHub repository.

## Setup

Set up a new private GitHub repository that you want to backup your Dynalist to.

The `PLAY_SESSION` cookie is required as input `dynalist_session_cookie` to this Action for authentication with https://dynalist.io. Be aware that this cookie allows read and write access to all your Dynalist data and should be handled with care.

Log in to https://dynalist.io and retrieve the value of the `PLAY_SESSION` cookie through your browser dev tools. Save it [as a secret](https://docs.github.com/en/actions/reference/encrypted-secrets#creating-encrypted-secrets-for-a-repository) with key `DYNALIST_SESSION_COOKIE` in your repo.

The `PLAY_SESSION` cookie expires after 1 year so that the secret in this repo must be updated with the new value after that time so that the action keeps working.

Create a file `.github/workflows/backup.yml` in your private backup repository to automatically backup once a week:
```yml
on:
  push:
    branches:
      - main
  schedule:
    # Once a week, https://crontab.guru/#0_0_*_*_0
    - cron: "0 0 * * 0"

jobs:
  dynalist_backup:
    runs-on: ubuntu-latest
    steps:
      - uses: jakob-ed/dynalist-backup@v1.0.0
        with:
          dynalist_session_cookie: ${{ secrets.DYNALIST_SESSION_COOKIE }}
```

For each run of the Action, a new commit containing the latest backup will be created. If you want to look at your backed up Dynalist data, download the ZIP and uncompress it.

## Disclaimer

This is not an official Dynalist tool, use at your own risk.
