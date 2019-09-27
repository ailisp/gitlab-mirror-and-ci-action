# Mirror to GitLab and trigger GitLab CI

A GitHub Action that mirrors all commits to GitLab, triggers GitLab CI, and returns the results back to GitHub. 

This action uses active polling to determine whether the GitLab pipeline is finished. This means our GitHub Action will run for the same amount of time as it takes for GitLab CI to finish the pipeline. 

## Difference with original version
This fork add support to trigger CI with pull request from a fork of the repo.

## Example workflow

This is an example of a pipeline that uses this action:

```workflow
name: Mirror and run GitLab CI

on: [pull_request]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v1
    - name: Mirror + trigger CI
      uses: ailisp/gitlab-mirror-and-ci-action@master
      with:
        args: "<namespace>/<repository>"
      env:
        GITLAB_HOSTNAME: "gitlab.com"
        GITLAB_USERNAME: "ailisp"
        GITLAB_PASSWORD: ${{ secrets.GITLAB_PASSWORD }} # Generate here: https://gitlab.com/profile/personal_access_tokens
        GITLAB_PROJECT_ID: "<GitLab project ID>"
        GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }} # https://help.github.com/en/articles/virtual-environments-for-github-actions#github_token-secret
```

Be sure to define the `GITLAB_PASSWORD` secret.
