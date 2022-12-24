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
Node dependencies, and then runs the following tools:

- [black](https://github.com/psf/black)
- [isort](https://pycqa.github.io/isort/)
- [autoflake](https://github.com/PyCQA/autoflake)
- [pyleft](https://github.com/NathanVaughn/pyleft)
- [pyright](https://github.com/Microsoft/pyright)
- [pyproject-flake8](https://github.com/csachs/pyproject-flake8)

This workflow attempts to determine the Python version to use, by parsing the
`Dockerfile` in the repository. This is either by using a `FROM` image with
`python` in the name, or explicitly setting an environment variable called
`PYTHON_VERSION`. Whatever the last found version specifier will be used.
