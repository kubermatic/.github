# Usage guidelines

## Prevent Unauthorized labels on PRs

This workflow is used to prevent unauthorized labels from being added to PRs. The workflow will always prevent the following labels from being added manually to PRs:

- `lgtm`
- `approved`

The "labels" input can be used to specify additional labels to prevent. It's a comma-separated list of labels for example `needs-rebase,needs-rebase-approved,needs-rebase-lgtm`. **NOTE: For additional labels, make sure to update the `if` condition in the workflow file to include the new labels.**

### Example usage

Either use Rulesets for organization/repository level configuration, or copy the workflow file to your repository at `.github/workflows/prevent-unauthorized-labels.yml`.

```yaml
name: Prevent Unauthorized Labels

on:
  pull_request_target:
    types: [opened, synchronize, reopened, labeled, unlabeled]

jobs:
  check_unauthorized_labels:
    if: contains(github.event.pull_request.labels.*.name, 'lgtm') || contains(github.event.pull_request.labels.*.name, 'approved')
    uses: kubermatic/.github/.github/workflows/prevent-unauthorized-labels.yml@main
    with:
      pr_number: ${{ github.event.pull_request.number }}
      repository: ${{ github.repository }}
      # labels: "bug,feature"
    secrets:
      GH_TOKEN: ${{ secrets.GITHUB_TOKEN }}
    permissions:
      # Write permissions are required to remove unauthorized labels
      pull-requests: write
      # Read permissions are required to get the PR labels and timeline
      contents: read
```
