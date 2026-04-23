# gha-setup-workspace

This GitHub Action sets up the job workspace by checking the job type, optionally checking out the source code, and handling artifacts based on the job type.

_*NOTE: This workflow is opinionated and meets the needs of its author. It is provided publicly as a reference for others to use and modify as needed.*_

The `gha-setup-workspace` action performs the following tasks:
- Sets additional GitHub Actions environment variables.
- Checks the checkout type (source or artifact).
- Checks out the source code if checkout_artifact is `false`.
- Downloads the artifact if checkout_artifact is `true`.

## Usage
See below for inputs, outputs, and examples.

### Inputs

- `checkout_artifact` (optional): Whether to checkout artifact instead of source. Value must be `true` or `false`. When set to `true`, this action downloads artifacts using the default behavior of `actions/download-artifact`.
- `checkout_fetch_depth` (optional): The number of commits to fetch. Change to `0` if performing a merge.

### Artifact selection behavior

When `checkout_artifact` is `true`, this action does not specify an artifact name when downloading artifacts.
As a result, it relies on the default behavior of `actions/download-artifact`, which downloads all available artifacts for the workflow run when no name is provided.

If your workflow expects exactly one artifact to be present in the workspace, configure upstream jobs to upload only one artifact for that run.
If multiple artifacts are uploaded, all of them may be downloaded into the workspace.

### Artifact selection behavior

When `checkout_artifact` is `true`, this action does not specify an artifact name when downloading artifacts.
As a result, it relies on the default behavior of `actions/download-artifact`, which downloads all available artifacts for the workflow run when no name is provided.

If your workflow expects exactly one artifact to be present in the workspace, configure upstream jobs to upload only one artifact for that run.
If multiple artifacts are uploaded, all of them may be downloaded into the workspace.
## Outputs

- `is-artifact`: Checkout type is artifact.
- `is-source`: Checkout type is source.

### Examples

To use this action, add the following step to your workflow:

```yaml
name: CI

on: [push, pull_request]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Setup job workspace
        uses: ServerlessOpsIO/gha-setup-workspace@v1

  deploy:
    runs-on: ubuntu-latest
    needs:
      - build
    steps:
      - name: Setup job workspace
        uses: ServerlessOpsIO/gha-setup-workspace@v1
        with:
          checkout_artifact: true
```

## Contributing

Contributions are welcome! Please open an issue or submit a pull request for any changes.

## Contact

For any questions or support, please open an issue in this repository.
