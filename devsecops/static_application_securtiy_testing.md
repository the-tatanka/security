# Static Application Security Testing (SAST)

<b>Semgrep</b> is a fast, open-source, static analysis tool for finding bugs and enforcing code standards at editor, commit, and CI time.

- Template Github actions config: https://semgrep.dev/docs/semgrep-ci/sample-ci-configs/#github-actions

- https://semgrep.dev/

- Semgrep rule sets: https://semgrep.dev/explore

```
  semgrep:
    runs-on: ubuntu-latest
    needs: [Build]
    steps:
      # Fetch project source
      - uses: actions/checkout@v2

      - uses: returntocorp/semgrep-action@v1
        id: semgrep
        continue-on-error: true
        with:
          config: >- # more at semgrep.dev/explore
            p/security-audit
            p/owasp-top-ten
            p/xss
            p/r2c
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

      # Upload findings to GitHub Advanced Security Dashboard [step 2/2]
      # - name: Upload SARIF file for GitHub Advanced Security Dashboard
      #   uses: github/codeql-action/upload-sarif@v1
      #   with:
      #     sarif_file: semgrep.sarif
      #   if: always()

      - name: Upload semgrep output
        uses: actions/upload-artifact@v1
        with:
          name: semgrep.sarif
          path: semgrep.sarif

      - name: Check on failures
        if: steps.semgrep.outcome != 'success'
        uses: actions/github-script@v3
        with:
          script: |
            core.setFailed('Detected semgrep findings')
```
