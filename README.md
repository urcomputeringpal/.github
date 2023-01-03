# Ur Computering Pal

:wave: https://urcomputeringpal.com

## Reusable Actions

### Workflow Failure Issues

Listens for `workflow_run` events and open issues when a workflow fails. Useful for being alerted to workflow failures before they interrupt the progress of multiple team members.

```yaml
name: open issues on workflow failure, close on success
on:
  workflow_run:
    # Configure the workflows to monitor here
    workflows:
      - test
      - release
      - etc
    types:
      - completed
jobs:
  workflow-failure-issues:
    uses: urcomputeringpal/.github/.github/workflows/workflow-failure-issues.yaml@main
    # Configure the type of events to listen for here
    if: |
      (
        github.event_name == 'schedule' ||
        github.event.workflow_run.head_branch == github.event.repository.default_branch
      )
```

### Renovate

Like dependabot, but customizable. Requires a private GitHub App be installed to grant the permissions necessary to update Actions workflows etc. [Create a new app](https://docs.github.com/en/developers/apps/creating-a-github-app) and configure the app permissions and your renovate.js as described in the [Renovate documentation](https://docs.renovatebot.com/modules/platform/github/#running-as-a-github-app).

```yaml
name: renovate
on:
  push:
    branches:
      - master
  workflow_dispatch: {}
  schedule:
    - cron:  '0 */6 * * *'
jobs:
  renovate:
    uses: urcomputeringpal/.github/.github/workflows/renovate.yaml@main
    with:
      renovate_app_slug: your-app-name
    secrets:
      RENOVATE_APP_ID: ${{ secrets.RENOVATE_APP_ID }}
      RENOVATE_APP_PEM: ${{ secrets.RENOVATE_APP_PEM }}
```