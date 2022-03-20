# Static Application Security Testing (SAST)

<b>Semgrep</b> is a fast, open-source, static analysis tool for finding bugs and enforcing code standards at editor, commit, and CI time.

- Template Github actions config: https://semgrep.dev/docs/semgrep-ci/sample-ci-configs/#github-actions

- https://semgrep.dev/

- Semgrep rule sets: https://semgrep.dev/explore

## Integration

1. Enable code scanning in your repository.

2. Create a new workflow named "semgrep.yml" in your ".github/workflows" directory.

3. Paste the example Semgrep action below.

4. Search out matching Semgrep rulesets depending on the frameworks or technologies you are using: https://semgrep.dev/explore

5. Add them to the action under "config:".

```
# For most projects, this workflow file will not need changing; you simply need
# to commit it to your repository.
#
# You may wish to alter this file to override the set of languages analyzed,
# or to provide custom queries or build logic.
#
name: "Semgrep"

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
    - name: Checkout repository
      uses: actions/checkout@v2

    - uses: returntocorp/semgrep-action@v1
      id: semgrep
      with:
        config: >- # more at semgrep.dev/explore
          p/security-audit
          p/owasp-top-ten
          p/xss
          p/r2c
          p/react
          p/secrets
          p/docker
          p/typescript
        generateSarif: "1"

      # == Optional settings in the `with:` block
      # Instead of `config:`, use rules set in Semgrep App.
      # Get your token from semgrep.dev/manage/settings.
      #   publishToken: ${{ secrets.SEMGREP_APP_TOKEN }}

      # Never fail the build due to findings on pushes.
      # Instead, just collect findings for semgrep.dev/manage/findings
      #   auditOn: push

      # Upload findings to GitHub Advanced Security Dashboard [step 1/2]
      # See also the next step.
      #   generateSarif: "1"

      # Change job timeout (default is 1800 seconds; set to 0 to disable)
      # env:
      #   SEMGREP_TIMEOUT: 300

    - name: Upload Semgrep output as artifact
      uses: actions/upload-artifact@v1
      with:
        name: semgrep.sarif
        path: semgrep.sarif
      if: always()

    # Upload findings to GitHub Advanced Security Dashboard [step 2/2]
    - name: Upload SARIF file for GitHub Advanced Security Dashboard
      uses: github/codeql-action/upload-sarif@v1
      with:
        sarif_file: semgrep.sarif
      if: always()
```

## Results evaluation

High findings must be fixed as soon as possible.

The findings can be seen at GitHub Security / Code scanning alerts.

False positives can be listed as findings. Therefore, an evaluation of the findings is required. In most cases expert knowledge regarding the used software is necessary.
