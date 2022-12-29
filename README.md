# github-actions

These are common GitHub Actions workflows used across multiple of
BellFlight's repositories.

## Usage

To use a workflow in another, reference it like so:

```yml
jobs:
  job1:
    uses: bellflight/github-actions/.github/workflows/<workflow>.yml@main
    with:
      ...
```

### AVR Build and Push Container

File: [`avr_build_and_push_container.yml`](.github/workflows/avr_build_and_push_container.yml)

This will build a Docker container, tag it with the date and time it was built,
and push it to GitHub Container Registry.
The container is only pushed when the workflow is run on a `push` to the `main` branch.

#### Inputs

- `container_name` (Required): Name to assign to container. This will be prefixed by `ghcr.io/bellflight/avr/`.
- `platforms`: Comma-seperated string of platforms to build the container for. Defaults to `linux/amd64,linux/arm64`.

### AVR Python Checks

File: [`avr_python_checks.yml`](.github/workflows/avr_python_checks.yml)

This installs Python dependencies using [Poetry](https://python-poetry.org),
then runs [Pre-Commit](https://pre-commit.com/). This utilizes the configuration
defined in the repository's `.pre-commit-config.yaml` file.

This workflow attempts to determine the Python version to use, by parsing the
`Dockerfile` in the repository. This is either by using a `FROM` image with
`python` in the name, or explicitly setting an environment variable called
`PYTHON_VERSION`. Whatever the last found version specifier will be used.

### AVR Python Tests

File: [`avr_python_tests.yml`](.github/workflows/avr_python_tests.yml)

This installs Python dependencies using [Poetry](https://python-poetry.org),
then runs [pytest](https://docs.pytest.org/) with the
[pytest-cov](https://pytest-cov.readthedocs.io/en/latest/) plugin.

This workflow attempts to determine the Python version to use, by parsing the
`Dockerfile` in the repository. This is either by using a `FROM` image with
`python` in the name, or explicitly setting an environment variable called
`PYTHON_VERSION`. Whatever the last found version specifier will be used.

#### Inputs

- `coverage_directory`: Directory to record coverage for. Defaults to `src`.


### Pre-Commit Update

File: [`pre-commit_update.yml`](.github/workflows/pre-commit_update.yml)

This updates the repositories referenced in the repository's
`.pre-commit-config.yaml` file and creates a pull request.

#### Inputs

- `target_branch`: Branch to checkout and create the pull request into. Defaults to `develop`.
