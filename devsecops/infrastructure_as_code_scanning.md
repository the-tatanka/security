## Infrastructure as Code (IaC) Scanning

Currently, <b>KICS</b> scanning supports configuration files for Ansible, AWS Cloudformation, Azure Resource Manager, Dockerfile, Google Deployment Manager, Kubernetes, OpenAPI, Terraform, and Helm.

<b>Semgrep also has IaC capabilities.</b> The corresponding semgrep packages must be configured, see SAST semgrep section.

- https://kics.io/

- KICS Github action: https://github.com/Checkmarx/kics-github-action

```
name: KICS IaC Scan

on:
  pull_request:

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v2

    - name: run kics Scan
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

    - name: Upload SARIF file
        uses: github/codeql-action/upload-sarif@v1
        with:
          sarif_file: kicsResults/results.sarif
```
