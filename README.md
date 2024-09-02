# gha-setup-workspace

This GitHub Action sets up the job workspace by checking the job type, optionally checking out the source code, and handling artifacts based on the job type.

_*NOTE: This workflow is opinionated and meets the needs of its author. It is provided publicly as a reference for others to use and modify as needed.*_

The `gha-setup-workspace` action performs the following tasks:
* Sets additional GitHub Actions environment variables.
* Checks the job type (build or deploy).
* Checks out the source code if the job type is not `deploy`.
* Determines the artifact name based on the provided override or generates a default name.
* Downloads the artifact if the job type is `deploy`.

## Inputs

- `job_type` (required): The type of job action is executing in. Must be either `build` or `deploy`.
- `artifact_name_override` (optional): Override the name of the artifact to use. Default is an empty string which determines the artifact name automatically. 
- `checkout_fetch_depth` (optional): The number of commits to fetch. Change to `0` if performing a merge.

## Outputs

- `artifact_name`: The name of the artifact downloaded.
- `job_type`: The type of job action is executing in.

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
        uses: serverlessops/gha-setup-workspace
        with:
          job_type: build

  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Setup job workspace
        uses: serverlessops/gha-setup-workspace
        with:
          job_type: deploy
```

## Contributing

Contributions are welcome! Please open an issue or submit a pull request for any changes.

## Contact

For any questions or support, please open an issue in this repository.

