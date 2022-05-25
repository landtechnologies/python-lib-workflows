# codeartifact-workflows
Public re-usable GitHub workflows for working with AWS CodeArtifact

# Usage
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
    uses: landtechnologies/codeartifact-workflows/.github/workflows/python_codeartifact_push.yml@main
    with:
      repository: YOUR_REPOSITORY_NAME
      domain: YOUR_DOMAIN_NAME
      domain_owner: YOUR_AWS_ACCOUNT_ID
    secrets:
      AWS_ACCESS_KEY_ID: YOUR_GITHUB_SECRET_FOR_ACCESS_KEY_ID
      AWS_SECRET_ACCESS_KEY: YOUR_GITHUB_SECRET_FOR_SECRET_ACCESS_KEY
```
