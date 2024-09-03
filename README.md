# gha-setup-workspace

This GitHub Action sets up the job workspace by checking the job type, optionally checking out the source code, and handling artifacts based on the job type.

_*NOTE: This workflow is opinionated and meets the needs of its author. It is provided publicly as a reference for others to use and modify as needed.*_

The `gha-setup-workspace` action performs the following tasks:
* Sets additional GitHub Actions environment variables.
* Checks the job of checkout (source or artifact)
* Checks out the source code if checkout_artifact is `false`.
* Determines the artifact name based on the provided override or generates a default name.
* Downloads the artifact if checkout_artifact is `true`
* .

## Inputs

- `artifact_name_override` (optional): Override the name of the artifact to use. Default is an empty string which determines the artifact name automatically. 
- `checkout_artifact` (optional): Whether to checkout artifact instead of source. Value must be `true` or `false`.
- `checkout_fetch_depth` (optional): The number of commits to fetch. Change to `0` if performing a merge.

## Outputs

- `artifact_name`: The name of the artifact downloaded.
- `is_artifact`: Checkout type is artifact.
- `is_source`: Checkout type is source.

## Usage

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
    steps:
      - name: Setup job workspace
        uses: ServerlessOpsIO/gha-setup-workspace@v1
        with:
          checkout_artifact: true
```

### Artifact naming

Both [ServerlessOpsIO/gha-setup-workspace](https://github.com/ServerlessOpsIO/gha-setup-workspace) and [ServerlessOpsIO/gha-store-artifacts](https://github.com/ServerlessOpsIO/gha-store-artifacts) use the same [utility action](https://github.com/ServerlessOpsIO/gha-artifact-name) to set an artifact name if `artifact_name_override` is not set. If your workflow sets _artifact_name_override_ for `ServerlessOpsIO/gha-setup-workspace` be sure to use that actions _artifact_name_ output to set _artifact_name_override_ for `ServerlessOpsIO/gha-store-artifacts`.

See the example below:

```yaml
- name: Setup job workspace
  id: setup-workspace
  uses: ServerlessOpsIO/gha-setup-workspace@v1
  with:
    artifact_name_override: 'my-artifact'


- name: Store Artifacts
  uses: ServerlessOpsIO/gha-store-artifacts@v1
  with:
    artifact_name_override: '${{ steps.setup-workspace.outputs.artifact_name }}'
```

## Contributing

Contributions are welcome! Please open an issue or submit a pull request for any changes.

## Contact

For any questions or support, please open an issue in this repository.
