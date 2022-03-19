# Dynamic Application Security Testing (DAST)

Dynamic Application Security Testing (DAST) examines applications for vulnerabilities like these in deployed environments.

<b>OWASP Zed Attack Proxy</b>:

- https://www.zaproxy.org/

- Fast / baseline scan (passive): https://www.zaproxy.org/docs/docker/baseline-scan/

- Full scan (active): https://www.zaproxy.org/docs/docker/full-scan/

DAST can analyze applications in two ways:

- Passive scan only (smoke tests). DAST executes ZAP’s Baseline Scan and doesn’t actively attack your application.

- Active scan - can be configured to also perform an active scan to attack your application and produce a more extensive security report.
