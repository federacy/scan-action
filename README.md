# Salus Security Scan Action 

This action utilizes [Salus](https://github.com/coinbase/salus) from Coinbase to run SAST and dependency scans. 

## Scanners supported

| Name | Language | 
| ---- | -------- | 
| [Bundle Audit](https://github.com/rubysec/bundler-audit) | Ruby |
| [Brakeman](https://github.com/presidentbeef/brakeman) | Ruby | 
| [npm audit](https://docs.npmjs.com/cli/audit) | JavaScript |
| [yarn audit](https://yarnpkg.com/lang/en/docs/cli/audit/) | JavaScript |
| [Gosec](https://github.com/securego/gosec) | Go | 
| [Bandit](https://github.com/coinbase/salus/blob/master/docs/scanners/bandit.md) | Python |
| [Cargo Audit](https://github.com/coinbase/salus/blob/master/docs/scanners/cargo_audit.md) | Rust |
| [semgrep](https://github.com/coinbase/salus/blob/master/docs/scanners/semgrep.md) | [Many](https://semgrep.dev/docs/status/) |
| [PatternSearch](https://github.com/coinbase/salus/blob/master/docs/scanners/pattern_search.md) | n/a (uses [Sift](https://sift-tool.org/)) | 

## Example usage

`.github/workflows/example.yml`

### Defaults

```
on: [push]

jobs:
  salus_scan_job:
    runs-on: ubuntu-latest
    name: Salus Security Scan Example
    steps:
    - uses: actions/checkout@v1
    - name: Salus Scan
      id: salus_scan
      uses: federacy/scan-action@0.1.4
```

### Single scanner

```
on: [push]

jobs:
  salus_scan_job:
    runs-on: ubuntu-latest
    name: Salus Security Scan Example
    steps:
    - uses: actions/checkout@v1
    - name: Salus Scan
      id: salus_scan
      uses: federacy/scan-action@0.1.4
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
    name: Salus Security Scan Example
    steps:
    - uses: actions/checkout@v1
    - name: Salus Scan
      id: salus_scan
      uses: federacy/scan-action@0.1.4
      with:
          enforced_scanners: "none"
```

### Custom configuration

```
on: [push]

jobs:
  salus_scan_job:
    runs-on: ubuntu-latest
    name: Salus Security Scan Example
    steps:
    - uses: actions/checkout@v1
    - name: Salus Scan
      id: salus_scan
      uses: federacy/scan-action@0.1.4
      env:
        SALUS_CONFIGURATION: "file://../salus-configuration.yaml file://config/pattern_search.yaml"
```

## Inputs

| attribute | description | default | options |
| --------- | ----------- | ------- | ------- |
| active_scanners | Scanners to run | all | Brakeman, PatternSearch, BundleAudit, NPMAudit, GoSec |
| enforced_scanners | Scanners that block builds | all | Brakeman, PatternSearch, BundleAudit, NPMAudit, GoSec |
| report_uri | Where to send Salus reports | file://salus-report.json | Any URI |
| report_format | What format to use for report | json | json, yaml, txt |
| report_verbosity | Whether to enable a verbose report | true | true, false |
| salus_configuration | Where to find Salus configuration | file://../salus-configuration.yaml | Any URI |

Note: active_scanners and enforced_scanners must be yaml formatted for Salus configuration file.

## Outputs

None.

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

## Contributors

[Chris Czub](https://github.com/zbuc)   
[Andre Azzolini](https://github.com/apazzolini)   
[Quentin Le Sceller](https://github.com/quentinlesceller)   
