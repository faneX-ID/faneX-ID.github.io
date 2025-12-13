# faneX-ID Documentation Website

This repository contains the documentation website for faneX-ID.

## Setup

### Required Secrets

To fetch data from the private `faneX-ID/core` repository, you need to set up a GitHub Personal Access Token (PAT) with the following permissions:

- `repo` (Full control of private repositories)
- `read:org` (Read org and team membership)

1. Create a Personal Access Token (PAT) on GitHub
2. Add it as a secret named `CORE_REPO_TOKEN` in this repository's settings
3. The `fetch-releases.yml` workflow will use this token to access the private core repository

## Workflows

### fetch-releases.yml

This workflow:
- Fetches latest releases from all faneX-ID repositories
- Fetches version information from the private `faneX-ID/core` repository
- Fetches build status from the core repository
- Updates the documentation data files automatically

Runs:
- Every 6 hours (scheduled)
- On manual trigger (workflow_dispatch)
- When a release is published (repository_dispatch)

## Data Files

The workflow generates the following data files in `docs/data/`:

- `releases.json` - Latest releases from all repositories
- `versions.json` - Version information from core repository
- `builds.json` - Build status from core repository

These files are used by the documentation pages to display current information.
