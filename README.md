# Federacy Salus Security Scan Action 

This action utilizes [Salus](https://github.com/coinbase/salus) from Coinbase to run SAST and dependency scans. 

Reports can optionally be sent to [SecureDevelopment by Federacy](https://www.securedevelopment.com) for analysis. 

Steps to send reports:

1. Create free account on https://www.securedevelopment.com
2. Click 'Applications' in navbar
3. Click 'Create Application'
4. Copy example job to your workflow

## Example usage

### Defaults

```
on: [push]

jobs:
  salus_scan_job:
    runs-on: ubuntu-latest
    name: Federacy Salus Security Scan Example
    steps:
    - uses: actions/checkout@v1
    - name: Federacy Salus Scan
      id: salus_scan
      uses: federacy/scan-action@v1
```

### Single scanner

```
on: [push]

jobs:
  salus_scan_job:
    runs-on: ubuntu-latest
    name: Federacy Salus Security Scan Example
    steps:
    - uses: actions/checkout@v1
    - name: Federacy Salus Scan
      id: salus_scan
      uses: federacy/scan-action@v1
      with:
          active_scanners: "\n  - Brakeman"
          enforced_scanners: "\n  - Brakeman"
```

### No enforced scanners 

```
on: [push]

jobs:
  salus_scan_job:
    runs-on: ubuntu-latest
    name: Federacy Salus Security Scan Example
    steps:
    - uses: actions/checkout@v1
    - name: Federacy Salus Scan
      id: salus_scan
      uses: federacy/scan-action@v1
      with:
          enforced_scanners: "none"
```

## Github Environment Variables

Stored in custom_info of a Salus scan.

| Key | Github Variable | Description |
| --- | ----------------- | ----------- |
| sha1    | GITHUB_SHA | Hash of last commit in build |
| reponame | GITHUB_REPOSITORY | Name of repository |
| ref | GITHUB_REF | Ref that triggered flow (branch or tag) |
| ci_username | GITHUB_ACTOR | Github username of user who triggered build |
| github_action | GITHUB_ACTION | Name of the action |
| github_workflow | GITHUB_WORKFLOW | Name of the workflow |
| github_event_name | GITHUB_EVENT_NAME | Name of the event that triggered workflow |
| github_event_path | GITHUB_EVENT_PATH | Path of event payload |
| github_workspace | GITHUB_WORKSPACE | Workspace directory path |
| github_head_ref | GITHUB_HEAD_REF | Ref of the head repository, if forked |
| github_base_ref | GITHUB_BASE_REF | Ref of the base repository, if forked |
| github_home | HOME | Path to home directory used by Github |

## Inputs

| attribute | description | default | options |
| --------- | ----------- | ------- | ------- |
| active_scanners | Scanners to run | all | Brakeman, PatternSearch, BundleAudit, NPMAudit, GoSec |
| enforced_scanners | Scanners that block builds | all | Brakeman, PatternSearch, BundleAudit, NPMAudit, GoSec |
| report_uri | Where to send Salus reports | file://../salus-report.json | Any URI |
| report_format | What format to use for report | json | json, yaml, txt |
| report_verbosity | Whether to enable a verbose report | true | true, false |
| configuration_file | Location of config file in repo (overrides all other parameters except salus_executor) | "" | Any filename |

Note: active_scanners and enforced_scanners must be yaml formatted for Salus configuration file.

## Outputs

None.
