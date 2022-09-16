# python-lib-workflows
Public re-usable GitHub workflows for working with python libraries which autorelease with bumpversion and publish to AWS CodeArtifact

# Usage
These reusable workflows are designed to run in sequence and take a `CUSTOM_GITHUB_TOKEN` to use in place of the standard CI token. This is because actions like commits, tags, releases, etc. performed by the CI token do not trigger other workflows as you would expect.

## bump_version.yml
This sample workflow will bump the version using bump2version, commit the new version, tag the commit and push.
Requires `new_version` and a section for `[bumpversion:part:auto]` in `.bumpversion.cfg` to enable automatic release.
```yaml
name: Bump version

on:
  push:
    branches:
      - main

jobs:
  bump_version:
    uses: landtechnologies/python-lib-workflows/.github/workflows/bump_version.yml@main
    with:
      git_name: YOUR_GIT_USER
      git_email: YOUR_GIT_EMAIL
    secrets:
      CUSTOM_GITHUB_TOKEN: ${{ secrets.YOUR_CUSTOM_GITHUB_TOKEN }}
```

## check_bumpversion_config.yml
This sample workflow will enforce that your `.bumpversion.cfg` meets the requirements for our automatic release.
```yaml
name: PR checks

on:
  pull_request:
    branches:
      - main

jobs:
  pr_checks:
    uses: landtechnologies/python-lib-workflows/.github/workflows/check_bumpversion_config.yml@main
```

## package_release.yml
This sample workflow will create a new GitHub release with automatic release notes when a `v*` tag is pushed.
```yaml
name: Create Release

on:
  push:
    tags:
      - 'v*' # Push events to matching v*, e.g. v1.0.0

jobs:
  package_release:
    uses: landtechnologies/python-lib-workflows/.github/workflows/package_release.yml@main
    secrets:
      CUSTOM_GITHUB_TOKEN: ${{ secrets.YOUR_CUSTOM_GITHUB_TOKEN }}
```

## python_codeartifact_push.yml
This sample workflow will trigger a build and push to CodeArtifact when a new
GitHub release is published.
```yaml
name: Your library deploy workflow
on:
  release:
    types: [published]
jobs:
  build-n-publish:
    uses: landtechnologies/python-lib-workflows/.github/workflows/python_codeartifact_push.yml@main
    with:
      repository: YOUR_REPOSITORY_NAME
      domain: YOUR_DOMAIN_NAME
      domain_owner: YOUR_AWS_ACCOUNT_ID
    secrets:
      AWS_ACCESS_KEY_ID: YOUR_GITHUB_SECRET_FOR_ACCESS_KEY_ID
      AWS_SECRET_ACCESS_KEY: YOUR_GITHUB_SECRET_FOR_SECRET_ACCESS_KEY
```
