# Slough - GitHub Actions - Workflows - Container projects

This repository contains GitHub Action workflows for Container projects. It contains two parts:

-   CI: a workflow to lint the containerfile with Hadolint
-   CD: a workflow to build and push the container to Docker Hub

It contains a `workflow.yml` that contains both parts.

## CI

Within the CI workflow for container projects, the containerfile gets linted with Hadolint.

To use CI from this project, use the following workflow in `.github/workflows/filename.yml`:

```yaml
jobs:
  ci:
    name: CI
    uses: DarylStark/slough-gha-wf-container-projects/.github/workflows/ci.yml@v1
  with:
    container-file: src/Dockerfile
```

The following inputs are available for this workflow:

-   `container-file`: *not required*: the location for the containerfile, relative to the project root. By default, this is `src/Dockerfile`

## CD

To use CD from this project, use the following workflow in `.github/workflows/filename.yml`:

```yaml
jobs:
  cd:
    name: CD
    uses: DarylStark/slough-gha-wf-container-projects/.github/workflows/cd.yml@v1
    with:
      container-tags: my-container:latest
    secrets:
      DOCKER_USERNAME: ${{ secrets.DOCKER_USERNAME }}
      DOCKER_PASSWORD: ${{ secrets.DOCKER_PASSWORD }}
```

The tags for the container will be automatically generated based on the following inputs:

-   `container-repository`: the repository where the container will be pushed
-   `container-name`: the name for the image. If not given, the name of the repository will be used
-   `version`: the version for the image

The following inputs are available for this workflow:

-   `container-repository`: *required*: the repository where the container will be pushed
-   `container-name`: *not required*: the name for the image. If not given, the name of the repository will be used
-   `version`: *not required*: the version for the image
-   `container-file`: *not required*: the location for the containerfile, relative to the project root. By default, this is `src/Dockerfile`
-   `container-context`: *not required*: the location for the context of the build. By default, this is `src/`
-   `platforms`: *not required*: the platforms for which to build this container in a comma seperated list. By default, this is `linux/arm64,linux/amd64`

The following secrets should be set for this workflow:

-   `DOCKER_USERNAME`: _required_: the username to log in to Docker Hub
-   `DOCKER_PASSWORD`: _required_: the password to log in to Docker Hub

## Full workflow

To use CI and CD in one step, you can use the followig workflow in `.github/workflows/filename.yml`:

```yaml
---
on:
  push:

jobs:
  container-project-workflow:
    name: Container Project
    uses: DarylStark/slough-gha-wf-container-projects/.github/workflows/workflow.yml@v1
    with:
      enable-ci: true
      enable-cd: ${{ github.ref_name == 'main' || github.ref_name == 'dev' }}
      container-repository: my-docker-repo
      version: 2.1.2
    secrets:
      DOCKER_USERNAME: ${{ secrets.DOCKER_USERNAME }}
      DOCKER_PASSWORD: ${{ secrets.DOCKER_PASSWORD }}
```

This will start the CI and CD steps of the workflow. The CD step will only be executed when the branch is `main` or `dev`. The tags will be automatically generated for the version and for the branch (on `main` it will generate `latest`, on `dev` it will generate `latest-dev`). The container will be pushed to the repository `my-docker-repo` with the name of the repository and the version as the tag.