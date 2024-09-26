# Slough - GitHub Actions - Workflows - Container projects

This repository contains GitHub Action workflows for Container project.

## CI

Within the CI workflow for container projects, the containerfile gets linted with Hadolint.

To use CI from this project, use the following workflow in `.github/workflows/filename.yml`:

```yaml
jobs:
  ci:
    name: CI
    uses: DarylStark/slough-gha-wf-container-projects/.github/workflows/ci-container.yml@main
```

The following inputs are available for this workflow:

-   `container-file`: *not required*: the location for the containerfile, relative to the project root. By default, this is `src/Dockerfile`

## CD

To use CD from this project, use the following workflow in `.github/workflows/filename.yml`:

```yaml
jobs:
  cd:
    name: CD
    uses: DarylStark/slough-gha-wf-container-projects/.github/workflows/cd-container.yml@main
    secrets:
      DOCKER_USERNAME: ${{ secrets.DOCKER_USERNAME }}
      DOCKER_PASSWORD: ${{ secrets.DOCKER_PASSWORD }}
```

The following inputs are available for this workflow:

-   `container-file`: *not required*: the location for the containerfile, relative to the project root. By default, this is `src/Dockerfile`
-   `container-tags`: *not required*: the tags for this container
-   `platforms`: *not required*: the platforms for which to build this container in a comma seperated list. By default, this is `linux/arm64,linux/amd64`

The following secrets should be set for this workflow:

-   `DOCKER_USERNAME`: _required_: the username to log in to Docker Hub
-   `DOCKER_PASSWORD`: _required_: the password to log in to Docker Hub