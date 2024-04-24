# github-actions
Reusable github workflows/actions

### Examples of usage

Run maven deploy when pushing to main branch
```
name: Compile, run unit tests and publish package to Github Packages

on:
  push:
    branches:
      - main

jobs:
  compile-test-publish:
    uses: tho-h/github-actions/.github/workflows/maven.yml@main
    with:
      maven-command-attributes: deploy
    secrets:
      token: ${{ secrets.GITHUB_TOKEN }}
    permissions:
      packages: write
      contents: read
```

Run maven verify when pushing to another branch
```
name: Compile and run unit tests

on:
  push:
    branches-ignore:
      - main

jobs:
  compile-test:
    uses: tho-h/github-actions/.github/workflows/maven.yml@main
    with:
      maven-command-attributes: verify
    secrets:
      token: ${{ secrets.GITHUB_TOKEN }}
    permissions:
      contents: read
```

Publish version (according to the name of the tag) when creating release
```
name: Set version, compile, run unit tests and publish package to Github Packages

on:
  release:
    types: created

jobs:
  set-version-publish-release:
    uses: tho-h/github-actions/.github/workflows/set-version-publish-release.yml@main
    with:
      version: ${{ github.event.release.tag_name }}
    secrets:
      token: ${{ secrets.GITHUB_TOKEN }}
    permissions:
      contents: read
      packages: write
```