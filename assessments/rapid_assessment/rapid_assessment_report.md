# Rapid assessment for \<REPLACE with service name\>

| Metadata                    | Description     |
| --------------------------- | --------------- |
| Service name                |                 |
| Service / product owner     |                 |
| Service data classification | \<Leave empty\> |
| Highest risk impact         | \<Leave empty\> |
| RRA responsibles            | \<Leave empty\> |
| Status                      | \<Leave empty\> |

## Service notes

<i>How does the service work? Do we have diagrams, demos, examples? Is the service in production yet?</i>

<i>Please describe (or link) the items.</i>

### Questionnaire

<i>Questionnaire does not need to be filled in. It can be used if the documentation of the service is not complete, regarding security.</i>

<i>If there are questions that do not apply or cannot be answered, feel free to skip them or mark them as “N/A” (non-applicable).</i>

#### Overview

| No. | Questions                                                                                                                 | Answers | Comments |
| --- | ------------------------------------------------------------------------------------------------------------------------- | ------- | -------- |
| 1.1 | What is the application's primary business purpose?                                                                       |         |          |
| 1.2 | What key benefits does the application offer its users?                                                                   |         |          |
| 1.3 | What is the most sensitive data element that will be handled by the system?                                               |         |          |
| 1.4 | What other large engagements/clients have you supported with this application?                                            |         |          |
| 1.5 | Please describe the technology stack of the application and infrastructure and whether each would be SaaS or self-hosted. |         |          |

#### Application Security

| No.  | Questions                                                                                                                                                                              | Answers | Comments |
| ---- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------- | -------- |
| 2.1  | Have you performed internal security audits of your code or application that, at a minimum, addressed the OWASP Top 10? If so, please provide a description of the review and results. |         |          |
| 2.2  | Has a security audit been performed by an external third party? If so, who performed this audit and are the results available?                                                         |         |          |
| 2.3  | How do you protect Catena-X data that will be stored on your servers or within your applications?                                                                                      |         |          |
| 2.4  | Do you have Secure Coding standards?                                                                                                                                                   |         |          |
| 2.5  | Do you perform regular penetration testing, regular security scanning of your application and/or infrastructure?                                                                       |         |          |
| 2.6  | Are secrets (password, api keys, etc) stored in a reversible manner within your application?                                                                                           |         |          |
| 2.7  | What options do you support for customer authentication (SAML, OIDC, 2 factor, certificates, passwords, etc.)?                                                                         |         |          |
| 2.8  | Will source code for the application be available?                                                                                                                                     |         |          |
| 2.9  | Do developers follow a security checklist prior to releasing new versions of the service to production? If yes, please describe (or link) the items covered in the checklist.          |         |          |
| 2.10 | What data input paths does the application support?                                                                                                                                    |         |          |
| 2.11 | What data output paths does the application support?                                                                                                                                   |         |          |
| 2.12 | What data input validation controls are in place?                                                                                                                                      |         |          |

#### Network-based Security

| No. | Questions                                                                                                                                                    | Answers | Comments |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------- | -------- |
| 3.1 | Please discuss any significant controls you employ at a network level for firewalling, intrusion detection/prevention, web application firewalls or similar. |         |          |
| 3.2 | Please discuss your level of support for encrypted transport (TLS, SSL, etc).                                                                                |         |          |
| 3.3 | Is your network accessible from the internet on insecure or clear-text protocols such as: telnet, snmp, smb/cifs, etc?                                       |         |          |
| 3.4 | Do you monitor and mitigate DDoS attacks? If yes, please describe how.                                                                                       |         |          |

#### Host-based Security

| No. | Questions                                                                               | Answers | Comments |
| --- | --------------------------------------------------------------------------------------- | ------- | -------- |
| 4.1 | Please discuss any significant endpoint controls you employ to ensure server hardening. |         |          |

#### Vulnerability Management

| No. | Questions                                                           | Answers | Comments |
| --- | ------------------------------------------------------------------- | ------- | -------- |
| 5.1 | Please discuss your approach to vulnerability and patch management. |         |          |

#### User Security

| No. | Questions                                                                                                                                                         | Answers | Comments |
| --- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------- | -------- |
| 6.1 | Do you employ 2 factor authentication within your organization for admin access, code deploys or other sensitive transactions?                                    |         |          |
| 6.2 | Do you have procedures for handling access revocation and credential rotation?                                                                                    |         |          |
| 6.3 | Do you enforce password complexity (example: 8 chars or more, mix of alpha/numeric/special) for your users?                                                       |         |          |
| 6.4 | Do you employ tools for content monitoring and Data Leakage Protection?                                                                                           |         |          |
| 6.5 | Do you perform logins and sensitive data transfer over encrypted connections (TLS/SSL) only?                                                                      |         |          |
| 6.6 | Will user passwords or authentication secrets be available to any other users via any functionality (example, admin users can see clear text passwords of users)? |         |          |
| 6.7 | How do the end-users interact with the service?                                                                                                                   |         |          |

#### Cloud-hosting Security

| No. | Questions                                                                                                                       | Answers | Comments |
| --- | ------------------------------------------------------------------------------------------------------------------------------- | ------- | -------- |
| 7.1 | Which cloud hosting provider(s) do you employ?                                                                                  |         |          |
| 7.2 | Do you use any third party services or communicate with third parties from this application? If so, which data is communicated? |         |          |

## Data

<i>We want to know about all data the service will process, store or receive (and not just store). Any data the service can touch or see must be considered.</i>

| Data name | Classification (Public, Staff Conf., Workgroup Conf., Individual Conf.) | Comments |
| --------- | ----------------------------------------------------------------------- | -------- |
|           |                                                                         |          |
|           |                                                                         |          |

## Threats

<i>CIA per Data. How might an attacker benefit from capturing or modifying the Data?</i>

\<Leave empty\>

### \<REPLACE with Data name\>

Affecting confidentiality:

-

Affecting integrity:

-

Affecting availability:

-

## Risks

<i>Risk is commonly defined as: risk = impact \* likelihood</i>

\<Leave empty\>

## Recommendations

<i>Recommendations are ordered by how much risk we're taking by not implementing the recommendation.</i>

<i>“MAXIMUM is we need to fix this right now”, “HIGH is we need to fix this within a week”, “MEDIUM is we’ve to look at this in the next 3mo”, “LOW is this would be also interesting to do or look at”.</i>

\<Leave empty\>

## Appendix

- Rapid Risk Assessment: https://infosec.mozilla.org/guidelines/risk/rapid_risk_assessment.html

- Metrics: \<REPLACE with link to RRA metrics>
