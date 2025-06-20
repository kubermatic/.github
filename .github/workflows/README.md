# Usage guidelines

## Prevent Unauthorized LGTM/Approved labels on PRs

This workflow is used to prevent unauthorized LGTM/Approved labels from being added to PRs.

### Example usage

Either use Rulesets for organization/repository level configuration, or copy the workflow file to your repository at `.github/workflows/prevent-unauthorized-lgtm-approved.yml`.

```yaml
name: Prevent Unauthorized LGTM/Approved labels

on:
  pull_request_target:
    types: [opened, synchronize, reopened, labeled, unlabeled]

jobs:
  check_lgtm_approved_authorization:
    if: contains(github.event.pull_request.labels.*.name, 'lgtm') || contains(github.event.pull_request.labels.*.name, 'approved')
    uses: kubermatic/.github/.github/workflows/prevent-unauthorized-lgtm-approved.yml@main
    with:
      pr_number: ${{ github.event.pull_request.number }}
      repository: ${{ github.repository }}
    secrets:
      GH_TOKEN: ${{ secrets.GITHUB_TOKEN }}
    permissions:
      pull-requests: write
      contents: read
```
