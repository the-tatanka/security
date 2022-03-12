# Rapid Assessment

Resources:

- https://infosec.mozilla.org/guidelines/risk/rapid_risk_assessment.html

- https://wiki.mozilla.org/Security/Data_Classification

Often there is no time or resources available for a detailed security assessment.

Therefore, an security assessment is needed that can be performed very rapidly.

The most important assets in an application / service are the data. That is why this approach is based on the data processed, stored or simply accessible by services.

This approach is intended for analyzing and assessing services, not processes or individual controls.

## Necessary materials

It is recommended to have these things available for the rapid assessment:

- Metadata: Name of a person or/and team responsible for the service and an understanding / description of how the service works

- Data flow diagram (see STRIDE section) or architecture / system diagram

- Data: List the kind of data that will be processed or stored: secrets, credentials, public data, confidential data, and anything else that may be important for this service

### Metadata

General information about the service.

| Metadata                    | Description |
| --------------------------- | ----------- |
| Service name                |             |
| Service owner               |             |
| service data classification | Leave empty |
| Highest risk impact         | Leave empty |

Plus service notes - put any notes that you feel are relevant to the understanding of the service, security, etc. Ask the service owner what the service does and a little bit of how it works. Ensure that you understand the service well. You should be able to reformulate what the service does, and the service owner to agree on your formulation.

### Data

You will need to ask the team or service owner about what kind of data the service processes or stores.

Examples are:

- Specific configuration data

- User or service credentials, secrets

- User data

- etc.

| Data name | Classification | Comments |
| --------- | -------------- | -------- |
|           |                |          |
|           |                |          |

Set the data classification for each data - see data classification metric.
