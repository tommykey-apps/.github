# tommykey-apps/.github

Shared workflows, actions, and community health files for the `tommykey-apps` GitHub organization.

## Composite Actions

### `terraform-plan`

Runs `terraform init` + `terraform plan` against a directory. Designed for PR-time validation that catches configuration errors before merge.

**Inputs:**
- `working-directory` (default: `infra`) — Directory containing Terraform code
- `terraform-version` (default: `~1`) — Terraform version constraint

**Caller responsibility:**
- Run `actions/checkout@v4` before this action
- Configure AWS credentials (or other provider auth) before this action
- Set any `TF_VAR_*` env vars at the job level

**Example:**

```yaml
name: CI

on:
  pull_request:
    paths: [infra/**]

jobs:
  terraform-plan:
    runs-on: ubuntu-latest
    env:
      TF_VAR_github_token: ${{ secrets.GH_PAT }}  # repo-specific TF_VARs go here
    steps:
      - uses: actions/checkout@v4

      - uses: aws-actions/configure-aws-credentials@v4
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: ap-northeast-1

      - uses: tommykey-apps/.github/.github/actions/terraform-plan@v1
        with:
          working-directory: infra
```

## Versioning

- `v1.0.0` — annotated release tag
- `v1` — moving tag pointing at the latest `v1.x.y`

Consumers should pin to `@v1` to receive non-breaking updates automatically.

Breaking changes will be released as `v2`, etc.
