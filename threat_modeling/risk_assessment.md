# Risk Assessment

Resources:

- https://infosec.mozilla.org/guidelines/risk/likelihood_indicators.html

- https://infosec.mozilla.org/guidelines/risk/standard_levels.html

- https://infosec.mozilla.org/guidelines/assessing_security_risk.html

- https://cwe.mitre.org/cwss/cwss_v1.0.1.html#2.5.3

Risk is composed both of the impact when a risk is manifested as well as the likelihood that the risk will manifest. Impact can be assessed in a risk assessment and is primarily based on the data which the service handles. Likelihood on the other hand is primarily driven by the presence or absence of security controls in the service.

Risk is commonly defined as: <b>risk = impact \* likelihood</b>

## Impact

Impact is the potential result that can be produced by the weakness, assuming that the weakness can be successfully reached and exploited.

Assessing impact is a relatively finite, quantitative exercise:

- define the maximum amount of how much money we might lose

- how badly our reputation would be damaged

- how many employees would be unable to work

- etc.

Risk impact generally does not change quickly over time unless services and products are redesigned, large features are added, new types of data is processed, etc.

| Level   | Description                                                                                                                |
| ------- | -------------------------------------------------------------------------------------------------------------------------- |
| None    | There is no technical impact to the software being analyzed at all. In other words, this does not lead to a vulnerability. |
| Low     | Minimal control over the software being analyzed, or only access to relatively unimportant information can be obtained.    |
| Medium  | Moderate control over the software being analyzed, or access to moderately important information can be obtained.          |
| High    | Significant control over the software being analyzed, or access to critical information can be obtained.                   |
| Maximum | Complete control over the software being analyzed, to the point where operations cannot take place.                        |

## Likelihood

The likelihood that a vulnerability in the service will be exploited in a calendar year due to the absence of the security control.

| Level  | Description                                                                                                                                   |
| ------ | --------------------------------------------------------------------------------------------------------------------------------------------- |
| None   | An attacker has no chance of success; i.e., the issue is a "bug" because there is no attacker role, and no benefit to the attacker.           |
| Low    | An attacker probably would not target this weakness, or could have very limited chances of success.                                           |
| Medium | An attacker would probably target this weakness successfully, but the chances of success might vary, or require multiple attempts to succeed. |
| High   | It is highly likely that an attacker would target this weakness successfully, with a reliable exploit that is easy to develop.                |

## Risk

| Likelihood / Impact | None | Low | Medium | High | Critical |
| ------------------- | ---- | --- | ------ | ---- | -------- |
| None                |      |     |        |      |          |
| Low                 |      |     |        |      |          |
| Medium              |      |     |        |      |          |
| High                |      |     |        |      |          |

The risk levels also represent a simplified ISO 31000 equivalent (and are non-compliant with ISO 31000).

| Level   | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| ------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Maximum | This is the most important level, where the risk is especially great. <ul><li>Attention: Full attention from all concerned parties required. <li>Impact: High or maximum impact.</li></li><li>Effort: All resources engaged on fixing issues. Following standard/guidelines is required.</li><li>Risk acceptance: Rarely accepted as residual risk, must be discussed, and must be mitigated or remediated.</li><li>Exception time (SLA): Recommend fixing immediately.</li></ul> |
| High    | <ul><li>Attention: Full attention from all concerned parties required.</li><li>Impact: Medium, high or maximum impact.</li><li>Effort: Some key resources engaged on fixing the issue. Following standard/guidelines is required.</li><li>Risk acceptance: Risk must be discussed, and must at least be mitigated.</li><li>Exception time (SLA): Recommend remediation within 7 days.</li></ul>                                                                                   |
| Medium  | <ul><li>Attention: Attention from all concerned parties.</li><li>Impact: Low, medium or high impact.</li><li>Effort: Best effort. Following standard/guidelines is required.</li><li>Risk acceptance: Risk should be discussed, and at least mitigated.</li><li>Exception time (SLA): Recommend remediation within 90 days.</li></ul>                                                                                                                                             |
| Low     | <ul><li>Attention: Expected but not required.</li><li>Impact: Low or medium impact.</li><li>Effort: Best effort and best practices expected.</li><li>Risk acceptance: Risk may often be accepted as residual risk.</li><li>Exception time (SLA): Indefinitely.</li></ul>                                                                                                                                                                                                          |
