# Security in the SDLC

The Software Development Life Cycle (SDLC) is a structured process that enables the production of high-quality, low-cost software, in the shortest possible production time. The SDLC defines and outlines a detailed plan with stages, or phases, that each encompass their own process and deliverables.

The role of security in the SDLC - integrating security activities throughout the SDLC helps create more reliable software. By incorporating security practices and measures into the earlier phases of the SDLC, vulnerabilities are discovered and mitigated earlier, thereby minimizing overall time involved, and reducing costly fixes later in the life cycle.

With modern application security testing tools, it is easy to integrate security throughout the SDLC. In keeping with the ‘secure SDLC’ concept, it is vital that security assurance activities such as penetration testing, threat modeling, code review, and architecture analysis are an integral part of development efforts.

The primary advantages of pursuing a secure SDLC approach include:

- Security as a continuous concern

- Awareness of security considerations by stakeholders

- Early detection of flaws in the system

- Cost reduction as a result of early detection and resolution of issues

- Overall reduction of intrinsic business risks for the organization

## What are the SDLC models/methodologies?

Waterfall, Agile, Lean, Iterative, Spiral, V-Shaped

More or less, all models include these phases: Planning, Coding, Building, Testing, Release, Deploy, Operate, Monitor.

## Security SDLC best practices

### Planning

- <b>Threat Modeling</b> - Bring your application design weaknesses to light by exploring potential hacker exploits. Spot design flaws that traditional testing methods and code reviews might overlook.

### Coding

- <b>Pre-Commit Hooks</b> - Prevent you from committing sensitive information like credentials into your code management platform.

- <b>Peer reviews</b> - Protect the master/main branch and perform peer reviews through the pull requests.

- <b>IDE Security plugins</b> - are not worth the trouble (my opinion). Some of them check for secure coding standards.

### Building & Testing

- <b>Upload build artifact</b>

- <b>Static Application Security Testing (SAST)</b> - Analyze source code to find security vulnerabilities that make your organization’s applications susceptible to attack. Address security and quality defects in code while it is being developed, helping you accelerate development an increase overall security and quality.

- <b>Software Composition Analysis (SCA)</b> - Secure and manage open source risks in applications and containers. Manage security, quality, and license compliance risk that comes from the use of open source and third-party code in applications and containers.

- <b>Secret scanning</b>

### Release & Deploy

- <b>Infrastructure as Code (IaC)</b> - Also known as software-defined infrastructure, allows the configuration and deployment of infrastructure components faster with consistency by allowing them to be defined as a code and also enables repeatable deployments across environments.

- <b>Compliance as Code (CaC)</b> - Building compliance into development and operations, and checks into Continuous Delivery so that regulatory compliance becomes an integral part.

- <b>Release management</b>

### Operate

- <b>Dynamic Application Security Testing (DAST)<b> - Dynamic analysis evaluates an application while executing it to uncover issues with its runtime behavior.

- <b>Fuzzing</b> - Identify defects and zero-day vulnerabilities in services and protocols. Use black box fuzzer to efficiently and effectively discover and remediate security weaknesses in software. Web API fuzzing performs fuzz testing of API operation parameters. Fuzz testing sets operation parameters to unexpected values in an effort to cause unexpected behavior and errors in the API backend. This helps you discover bugs and potential security issues that other QA processes may miss.

- <b>Penetration Testing<b> - Penetration testing analysis helps you find and fix exploitable vulnerabilities in your server-side applications and APIs. Reduce your risk of a breach by identifying and exploiting business-critical vulnerabilities, before hackers do.

### Monitor

Not my area of expertise.

- Log collection

- SIEM

- Rollback
