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
        endsWith(github.ref, github.event.repository.default_branch)
      )
```