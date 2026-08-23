# docker-workflows

This repository is for managing github workflows for the purpose of automating the Continuous Integration of a Docker codebase.


## Usage

Place two files in your repository's `.github/workflows/` directory. Images are
published as `zot.soh.re/jmainguy/<image>`, include the required OCI labels, and
are signed by immutable digest with Notation.

release.yaml
```yaml

on:
  push:
    branches:
      - main

permissions:
  contents: write
  pull-requests: write

jobs:
  release-please:
    uses: Jmainguy/docker-workflows/.github/workflows/docker-release.yml@v4
    secrets:
      docker_username: ${{ secrets.DOCKER_USERNAME }}
      docker_password: ${{ secrets.DOCKER_PASSWORD }}
      notation_private_key: ${{ secrets.NOTATION_PRIVATE_KEY }}
      notation_certificate_chain: ${{ secrets.NOTATION_CERTIFICATE_CHAIN }}
      notation_ca_certificate: ${{ secrets.NOTATION_CA_CERTIFICATE }}
    with:
      docker_url: zot.soh.re
      registry_namespace: jmainguy
      image_name: ${{ github.event.repository.name }}
      authors: "Jonathan Seth Mainguy <jon@soh.re>"
      url: "https://github.com/Jmainguy/soh.re"
      source: "https://github.com/Jmainguy/soh.re"
      description: "Container for serving soh.re"
      title: "soh.re"
      licenses: "GPL-2.0"
      vendor: "Jmainguy"
      docker_context: docker/

```

ci.yml
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
    uses: Jmainguy/docker-workflows/.github/workflows/docker-ci.yml@v4
    with:
      docker_context: docker/
```
