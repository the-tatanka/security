# Container scanning

Your application’s Docker image may itself be based on Docker images that contain known vulnerabilities. By including an extra container scanning job in your pipeline that scans for those vulnerabilities

Container scanning is often considered part of Software Composition Analysis (SCA). SCA can contain aspects of inspecting the items your code uses. These items typically include application and system dependencies that are almost always imported from external sources, rather than sourced from items you wrote yourself.

Trivy is a simple and comprehensive scanner for vulnerabilities in container images, file systems, and Git repositories, as well as for configuration issues (IaC).

## Integration

1. Enable code scanning in your repository.

2. Create a new workflow named "trivy.yml" in your ".github/workflows" directory.

3. Paste the example Trivy action below.

4. Configure path to your Docker image location at "image-ref:".

```
# Depending on the location of your Docker container
# you need to change the path to the specific Docker registry.
#
name: "Trivy"

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

    # It's also possible to scan your private registry with Trivy's built-in image scan.
    # All you have to do is set ENV vars.
    # Docker Hub needs TRIVY_USERNAME and TRIVY_PASSWORD.
    # You don't need to set ENV vars when downloading from a public repository.
    # For public images, no ENV vars must be set.
    - name: Run Trivy vulnerability scanner
      uses: aquasecurity/trivy-action@master
      with:
        # Path to Docker image
        image-ref: 'docker.io/my-organization/my-app:${{ github.sha }}'
        format: 'sarif'
        output: 'trivy-results.sarif'
        exit-code: '1'
        ignore-unfixed: true
        severity: 'CRITICAL,HIGH'
      env:
        TRIVY_USERNAME: Username
        TRIVY_PASSWORD: Password

    - name: Upload Trivy scan results to GitHub Security tab
      uses: github/codeql-action/upload-sarif@v1
      with:
        sarif_file: 'trivy-results.sarif'
```

## Results evaluation

High findings must be fixed as soon as possible.

The findings can be seen at GitHub Security / Code scanning alerts.

False positives can be listed as findings. Therefore, an evaluation of the findings is required. In most cases expert knowledge regarding the used software is necessary.
