## Infrastructure as Code (IaC) Scanning

Currently, <b>KICS</b> scanning supports configuration files for Ansible, AWS Cloudformation, Azure Resource Manager, Dockerfile, Google Deployment Manager, Kubernetes, OpenAPI, Terraform, and Helm.

<b>Semgrep also has IaC capabilities.</b> The corresponding semgrep packages must be configured, see SAST semgrep section.

- https://kics.io/

- KICS Github action: https://github.com/Checkmarx/kics-github-action

## Integration

1. Enable code scanning in your repository.

2. Creat a new workflow named "kics.yml" in your ".github/workflows" directory.

3. Paste the example KICS action below.

4. KICS must be shown the paths to IaC files to be scanned. Add them to the action under "path:".

```
# For most projects, this workflow file will not need changing; you simply need
# to commit it to your repository.
#
# You may wish to alter this file to override the set of languages analyzed,
# or to provide custom queries or build logic.
#
name: "KICS"

on:
  push:
    branches: [ main, master ]
  pull_request:
    # The branches below must be a subset of the branches above
    branches: [ main, master ]
    paths-ignore:
      - '**/*.md'
      - '**/*.txt'
  schedule:
    - cron: '28 15 * * 3'

jobs:
  analyze:
    name: Analyze
    runs-on: ubuntu-latest
    permissions:
      actions: read
      contents: read
      security-events: write

    steps:
    - uses: actions/checkout@v2

    - name: KICS scan
      uses: checkmarx/kics-github-action@v1.3
        with:
          # Scanning two directories: ./terraform/ ./cfn-templates/ plus a single file
          path: 'terraform,cfn-templates,my-other-sub-folder/Dockerfile'
          # Fail on HIGH and MEDIUM severity results
          fail_on: high,medium
          # when provided with a directory on output_path
          # it will generate the specified reports file named 'results.{extension}'
          # in this example it will generate:
          # - results-dir/results.json
          # - results-dir/results.sarif
          output_path: kicsResults/
          output_formats: 'json,sarif'
          # If you want KICS to ignore the results and return exit status code 0 unless a KICS engine error happens
          # ignore_on_exit: results
          # GITHUB_TOKEN enables this github action to access github API and post comments in a pull request
          token: ${{ secrets.GITHUB_TOKEN }}
          enable_comments: true

    # Display the results in json format
    - name: Display kics results
      run: |
        cat kicsResults/results.json
      if: always()

    # Upload findings to GitHub Advanced Security Dashboard
    - name: Upload SARIF file for GitHub Advanced Security Dashboard
      uses: github/codeql-action/upload-sarif@v1
      with:
        sarif_file: kicsResults/results.sarif
      if: always()
```

## Results evaluation

High findings must be fixed as soon as possible.

The findings can be seen at GitHub Security / Code scanning alerts.

False positives can be listed as findings. Therefore, an evaluation of the findings is required. In most cases expert knowledge regarding the used software is necessary.
