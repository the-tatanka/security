# Security assessment

The security assessment describes a detailed <b>detailed</b> threat modeling process.

## Writing a good diagram

Having a good diagram is key to a good threat model.

The first step is to draw a whiteboard diagram of the flow of data in your component. It's the DATA flow you care about, NOT the code flow. Your threats come via data, NOT code.

Use the following elements:

- External Interactor: Is an element that is outside your area of control. It could be a user calling into an API, it could be another component (browser, user) but not one that's being threat modeled.

- Process: A process is simply some code (or microservice). It does NOT mean that it's a "process" as OS's call processes, instead it's just a collection of code.

- Data Store: A datastore is something that holds data.

- Data Flow: Represents the flow of data through the system.

- Trust Boundary: Occurs when one component doesn't trust the component on the other side of the boundary. There is always a trust boundary between elements running at different privilege levels.

- <i>Data</i>: Could be a file, a registry key or personal data. This element does not appear in the classic data flow diagram. However, I find it helpful to explicitly model certain security relevant data.

  - Data Stores store data
  - Processes process data.
  - Data Flows send Data.

## STRIDE

STRIDE stands for "Spoofing", "Tampering", "Repudiation", "Information disclosure", "Denial of service", and "Elevation of privilege".

| Threat                 | Property        | Definition                                               |
| ---------------------- | --------------- | -------------------------------------------------------- |
| Spoofing               | Authentication  | Impersonating something or someone else                  |
| Tampering              | Integrity       | Modifying data or code                                   |
| Repudiation            | Non-repudiation | Claiming to have not performed an action                 |
| Information Disclosure | Confidentiality | Exposing information to someone not authorized to see it |
| Denial of Service      | Availability    | Deny or degrade service to users                         |
| Elevation of Privilege | Authorization   | Gain capabilities without proper authorization           |

Essentially the idea is that you can classify all your threats according to one of the 6 STRIDE categories. But STRIDE is not a rigorous classification mechanism - there's a ton of overlap between the various categories (a successful Elevation of Privilege attack could result in Tampering of data, for instance). But it doesn't change the fact that it's an extremely useful mechanism for analyzing threats to a system.

It turns out that some STRIDE threats only apply to particular types of elements. If you think about it, it makes sense - for instance, an Elevation of Privilege threat doesn't apply to data stores (since a data store simply holds data, it operates at no privilege level).

For each element type, the following threats are considered valid:

| Element type      | S   | T   | R   | I   | D   | E   |
| ----------------- | --- | --- | --- | --- | --- | --- |
| External Entities | X   |     | X   |     |     |     |
| Processes         | X   | X   | X   | X   | X   | X   |
| Data Stores       |     | X   | ?   | X   | X   |     |
| Data Flows        |     | X   |     | X   | X   |     |
| Data              |     | X   |     | X   | X   |     |

- ?: Data stores often come under attack to allow for a repudiation attack to work (if you have a log located in a data store, the attacker might try to flood the data store with log entries to enable a repudiation attack. In addition, logs held in data stores are almost always the mitigation against a repudiation threat.

## STRIDE-per-element

We call this process of considering the element-specific threats to each element in the DFD "STRIDE-per-Element".

And the best part about it is that STRIDE/element allows people to produce competent threat models with relatively little training.

The STRIDE-per-element methodology ends up creating a fair number of threats, even a component with a relatively tiny diagram. The good news is that many/most of those threats aren't meaningful threats.

This analysis is the core of the threat model, and where the real work associated with the process takes place.

## Risk analysis

For each threat, the risk must be determined. The defined risk assessment methodology can be used for this purpose.

## Risk treatment

For each risk, the risk treatment option must be determined.

Risk treatment options:

- Avoid - deciding not to proceed with the activity that introduced the unacceptable risk, choosing an alternative more acceptable activity that meets business objectives, or choosing an alternative less risky approach or process.

- Reduce - implementing a strategy that is designed to reduce the likelihood or consequence of the risk to an acceptable level, where elimination is considered to be excessive in terms of time or expense.

- Transfer - implementing a strategy that shares or transfers the risk to another party or parties, such as outsourcing the management of physical assets, developing contracts with service providers or insuring against the risk. The third-party accepting the risk should be aware of and agree to accept this obligation.

- Accept - making an informed decision that the risk rating is at an acceptable level or that the cost of the treatment outweighs the benefit. This option may also be relevant in situations where a residual risk remains after other treatment options have been put in place. No further action is taken to treat the risk, however, ongoing monitoring is recommended.

In an ideal world, a risk treatment plan can be created. Unfortunately, this is usually unrealistic and creates more overhead than benefit.

What cannot be waived, however, is for each risk with the treatment option "reduce", to create issues / stories for the associated mitigations and to include the issue number in the security assessment report.

This ensures that the mitigations are in the backlog and are visible to the development team.

### Risk treatment plan

Effective risk treatment relies on attaining commitment from key practice stakeholders and developing realistic objectives and timelines for implementation.

For each risk identified in the risk assessment, detail the following:

- Specify the treatment option agreed - avoid, reduce, share/transfer or accept.

- Document the treatment plan - outline the approach to be used to treat the risk. Any relationships or interdependencies with other risks should also be highlighted.

- Assign an appropriate owner - who is accountable for monitoring and reporting on progress of the treatment plan implementation. Where the treatment plan owner and the risk owner are different, the risk owner has ultimate accountability for ensuring the agreed treatment plan is implemented.

- Specify a target resolution date - where risk treatments have long lead times, consider the development of interim measures. For example, it is unlikely to be acceptable for a residual risk to be rated ‘high' and to have a risk treatment with a resolution timeframe of two years.
