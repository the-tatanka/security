# Software Composition Analysis (SCA)

It typically looks for a lock file then performs a build to fetch upstream dependency information. In the case of containers, Dependency Scanning uses the compatible manifest and reports only these declared software dependencies (and those installed as a sub-dependency). Dependency Scanning can not detect software dependencies that are pre-bundled into the container’s base image.

To identify pre-bundled dependencies, use Container Scanning.

<b>OWASP Dependency-Check</b> will be used to check for vulnerable dependencies.

- https://github.com/jeremylong/DependencyCheck

- Github action: https://github.com/dependency-check/Dependency-Check_Action

- OWASP Dependency Check command line arguments (action calls Docker container from OWASP Dependency Check): https://jeremylong.github.io/DependencyCheck/dependency-check-cli/arguments.html

```
  dependency-check:
    runs-on: ubuntu-latest
    needs: [Build]
    steps:
      - uses: actions/checkout@v2

      - name: Dependency Check
        uses: dependency-check/Dependency-Check_Action@main
        id: dependencyCheck
        continue-on-error: true
        with:
          project: "test"
          path: "."
          format: "HTML"
          args: >
            --failOnCVSS 7
            --enableRetired

      - name: Dependency Check Results Upload
        uses: actions/upload-artifact@master
        id: dependencyCheckResults
        continue-on-error: true
        with:
          name: dependency-check-report
          path: ${{ github.workspace }}/reports

      - name: Check on failures
        if: steps.dependencyCheck.outcome != 'success' || steps.dependencyCheckResults.outcome != 'success'
        uses: actions/github-script@v3
        with:
          script: |
            core.setFailed('Detected critical and high vulnerabilities')
```

The <b>CycloneDX</b> module for Node.js is used to generate a Software Bill-of-Materials (BOM) containing an aggregate of all project dependencies.

- Github Action: https://github.com/CycloneDX/gh-node-module-generatebom

- Dev dependency: https://www.npmjs.com/package/@cyclonedx/bom

Only production dependencies are part of the BOM. The result is a JSON file called "bom.json".
