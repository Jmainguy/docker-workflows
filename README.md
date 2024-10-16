# golang-workflows

This repository is for managing github workflows for the purpose of automating the Continuous Integration of a Golang codebase.


## Usage

Place a two files in your repos .github/workflows/ directory

release-please.yaml
```yaml
name: Call Reusable Golang Release-Please Workflow

on:
  push:
    branches:
      - main

jobs:
  release-please:
    uses: Jmainguy/golang-workflows/.github/workflows/golang-release.yml@v1
    secrets:
      token: ${{ secrets.GITHUB_TOKEN }}
```

push.yml
```yaml
name: Call Reusable Golang CI Workflow

on:
  push:

jobs:
  golang-ci:
    uses: Jmainguy/golang-workflows/.github/workflows/golang-ci.yml@v1
```
