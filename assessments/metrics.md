# Metrics

This section contains all metrics which are used during the assessment.

## Data classification

| Data classification      | Description                                                                                                                                                                                                                                                                                          |
| ------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Public                   | Non-confidential data.                                                                                                                                                                                                                                                                               |
| Staff Confidential       | Staff only <ul><li>Information shared in internal meetings</li><li>Documentation on a wiki</li><li>Source code</li></ul>                                                                                                                                                                             |
| Groups Confidential      | Specific work groups only <ul><li>Service passwords/credentials</li><li>Proprietary or protected information, code, libraries from partners</li><li>Contracts or legal documents</li><li>Unannounced communication materials (dates, visuals, plans) for campaigns, product launches, etc.</li></ul> |
| Individuals Confidential | Specific individuals only <ul><li>Release signing keys</li><li>Specific partner conversations</li><li>Employee bank account information</li><li>User/personal passwords/credentials</li></ul>                                                                                                        |

## Imapct

| Level   | Description                                                                                                                |
| ------- | -------------------------------------------------------------------------------------------------------------------------- |
| None    | There is no technical impact to the software being analyzed at all. In other words, this does not lead to a vulnerability. |
| Low     | Minimal control over the software being analyzed, or only access to relatively unimportant information can be obtained.    |
| Medium  | Moderate control over the software being analyzed, or access to moderately important information can be obtained.          |
| High    | Significant control over the software being analyzed, or access to critical information can be obtained.                   |
| Maximum | Complete control over the software being analyzed, to the point where operations cannot take place.                        |

## Likelihood

| Level   | Description                                                                                                                                                                            |
| ------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| None    | Due to the absence of security controls, an attacker has no chance of success; i.e., the issue is a "bug" because there is no attacker role, and no benefit to the attacker.           |
| Low     | Due to the absence of security controls, an attacker probably would not target this weakness, or could have very limited chances of success.                                           |
| Medium  | Due to the absence of security controls, an attacker would probably target this weakness successfully, but the chances of success might vary, or require multiple attempts to succeed. |
| High    | Due to the absence of security controls, it is highly likely that an attacker would target this weakness successfully, with a reliable exploit that is easy to develop.                |
| Maximum | The absence of security controls will cause a risk.                                                                                                                                    |

## Risk

The risk levels also represent a simplified ISO 31000 equivalent (and are non-compliant with ISO 31000).

| Level   | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| ------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Maximum | This is the most important level, where the risk is especially great. <ul><li>Attention: Full attention from all concerned parties required. </li><li>Impact: High or maximum impact.</li><li>Effort: All resources engaged on fixing issues. Following standard/guidelines is required.</li><li>Risk acceptance: Rarely accepted as residual risk, must be discussed, and must be mitigated or remediated.</li><li>Exception time (SLA): Recommend fixing immediately.</li></ul> |
| High    | <ul><li>Attention: Full attention from all concerned parties required.</li><li>Impact: Medium, high or maximum impact.</li><li>Effort: Some key resources engaged on fixing the issue. Following standard/guidelines is required.</li><li>Risk acceptance: Risk must be discussed, and must at least be mitigated.</li><li>Exception time (SLA): Recommend remediation within 7 days.</li></ul>                                                                                   |
| Medium  | <ul><li>Attention: Attention from all concerned parties.</li><li>Impact: Low, medium or high impact.</li><li>Effort: Best effort. Following standard/guidelines is required.</li><li>Risk acceptance: Risk should be discussed, and at least mitigated.</li><li>Exception time (SLA): Recommend remediation within 90 days.</li></ul>                                                                                                                                             |
| Low     | <ul><li>Attention: Expected but not required.</li><li>Impact: Low or medium impact.</li><li>Effort: Best effort and best practices expected.</li><li>Risk acceptance: Risk may often be accepted as residual risk.</li><li>Exception time (SLA): Indefinitely.</li></ul>                                                                                                                                                                                                          |

| Likelihood / Impact | None | Low    | Medium | High    | Maximum |
| ------------------- | ---- | ------ | ------ | ------- | ------- |
| None                | Low  | Low    | Low    | Low     | Low     |
| Low                 | Low  | Low    | Low    | Low     | Medium  |
| Medium              | Low  | Low    | Medium | Medium  | High    |
| High                | Low  | Medium | High   | High    | Maximum |
| Maximum             | Low  | Medium | High   | Maximum | Maximum |
