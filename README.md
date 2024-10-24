# docker-workflows

This repository is for managing github workflows for the purpose of automating the Continuous Integration of a Docker codebase.


## Usage

Place a two files in your repos .github/workflows/ directory

release.yaml
```yaml
name: Release Docker Image

on:
  push:
    branches:
      - main

permissions:
  contents: write
  pull-requests: write

jobs:
  release-please:
    uses: Jmainguy/docker-workflows/.github/workflows/docker-release.yml@v2
    secrets:
      docker_username: ${{ secrets.DOCKER_USERNAME }}
      docker_password: ${{ secrets.DOCKER_PASSWORD }}
    with:
      docker_url: zot.soh.re
      image_name: ${{ github.event.repository.name }}
      authors: "Jonathan Seth Mainguy <jon@soh.re>"
      vendor: "Jmainguy"
      docker_context: docker/
```

push.yml
```yaml
name: Build and Scan Docker Image

on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main

permissions:
  actions: read
  contents: read
  security-events: write

jobs:
  docker-ci:
    uses: Jmainguy/docker-workflows/.github/workflows/docker-ci.yml@v2
    with:
      docker_context: docker/
```
