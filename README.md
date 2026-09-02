<p align="center">
  <a href="https://github.com/GuiBranco/github-status-action-v2"><img alt="github-status-action-v2 status" src="https://github.com/GuiBranco/github-status-action-v2/workflows/test/badge.svg"></a>
  <a href="https://github.com/GuiBranco/github-status-action-v2"><img alt="github-status-action-v2 status" src="https://github.com/GuiBranco/github-status-action-v2/workflows/build/badge.svg"></a>
  <a href="https://wakatime.com/badge/github/guibranco/github-status-action-v2"><img src="https://wakatime.com/badge/github/guibranco/github-status-action-v2.svg" alt="wakatime"></a>
</p>

# GitHub Status Action V2

Adds a status update to a commit. GitHub will always show the latest state of a context.

> **Warning**
>
>**Disclaimer** This version was created because the [original (V1)](https://github.com/Sibz/github-status-action) has not been updated by the creator for a while.
 
## Usage

### Inputs

 * `authToken` (required)  
 Use secrets.GITHUB_TOKEN or your own token if you need to trigger other workflows that use "on: status"'
 * `state` (required)  
 The status of the check should only be `success`, `error`, `failure`, `pending`, or `cancelled`.
 `cancelled` is accepted as an input but mapped to `error` before it reaches GitHub, since
 GitHub's commit-status API has no `cancelled` state of its own.
 * `context`  
 The context, is displayed as the name of the check
 * `description`  
 Short text explaining the status of the check
 * `owner`  
 The repository owner defaults to context github.repository_owner if omitted
 * `repository`  
 Repository, default to context github.repository if omitted
 * `sha`  
 SHA of commit to update status on, defaults to context github.sha  
 *If using `on: pull_request` use `github.event.pull_request.head.sha`*
 * `target_url`  
 Url to use for the details link. If omitted no link is shown.
 * `retries`  
 Number of retries after a failed attempt, so the step makes up to `retries + 1`
 requests in total (the same counting as `curl --retry N`). Defaults to `3`.
 Set to `0` to attempt the request once and fail.
 Only transient failures are retried: network errors, the per-attempt timeout
 firing, `408`, `429`, and any `5xx`. A `4xx` such as `403 Resource not
 accessible by integration` or `422` fails the step immediately, since another
 identical request cannot succeed.
 * `retryDelaySeconds`  
 Delay in seconds between retry attempts. Defaults to `5`. `0` is valid and
 retries with no delay.
 * `timeoutSeconds`  
 Per-attempt request timeout in seconds, applied as a fresh `AbortSignal` on
 every attempt. Defaults to `30`.
  
  ### Outputs
  None.

  ## Example
  ```yml
name: "test"
on: # run on any PRs and main branch changes
  pull_request:
  push:
    branches:
      - main

  jobs:
  test: # make sure the action works on a clean machine without building
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v3
    - name: Run the action # You would run your tests before this using the output to set state/desc
      uses: guibranco/github-status-action-v2@v1.1.7
      with: 
        authToken: ${{secrets.GITHUB_TOKEN}}
        context: 'Test run'
        description: 'Passed'
        state: 'success'
        sha: ${{github.event.pull_request.head.sha || github.sha}}
```
