# Threat modeling

Resources:

- The article here is copied/pasted and shortened from this series of articles: https://docs.microsoft.com/en-us/archive/blogs/larryosterman/some-final-thoughts-on-threat-modeling

You threat model to identify threats to your component, which then lets you know where you need to concentrate your resources.

<b>It doesn't solve the problem.</b>

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

## Rules of thumb

While it can be easy to find threats, it is important to realize that all threats have real-world consequences for the development team.

Concentrate your efforts on those where the attacker can cause real damage.

To help you guide your thinking about what kinds of threats deserve mitigation, here are some rules of thumb that you can use while performing your threat modeling.

1. If the data hasn’t crossed a trust boundary, you don’t really care about it.

2. If the threat requires that the attacker is ALREADY running code on the client at your privilege level, you don’t really care about it.

3. If your code runs with any elevated privileges (even if your code runs in a restricted svchost instance) you need to be concerned.

4. If your code invalidates assumptions made by other entities, you need to be concerned.

5. If your code listens on the network, you need to be concerned.

6. If your code retrieves information from the internet, you need to be concerned.

7. If your code deals with data that came from a file, you need to be concerned (these last two are the inverses of rule #1).

8. If your code is marked as safe for scripting or safe for initialization, you need to be REALLY concerned.

For more information on the rules, see https://docs.microsoft.com/en-us/archive/blogs/larryosterman/threat-modeling-again-threat-modeling-rules-of-thumb

## Security concept / assessment report

A security concept or security assessment is a living document - as you change the design, you need to go back and update your threat model to see if any new threats have arisen since you started.

What goes into the security assessment report:

1. Project information

   1. Overview of project features

   2. List of contacts for questions

   3. Documentation materials - State the reviewed documentation

   4. Relevant requirements - Refer to the relevant requirements/ policies / frameworks / regulations (PCI DSS, ISO 27001, ASVS, etc.)

2. Management summary - Highlight the key findings and recommendations

3. Assessment Methodology - Document the used methodology

4. Data flow diagram

   1. DFD element-specific descriptions - A one or two paragraph description of your component and what it does (it helps an outsider to understand your diagram), similarly

5. Threat Analysis - For each threat that you call out in the threat analysis, you should include the associated mitigation (add issue number of threat or mitigation)

   1. Data flows

   2. External interactors

   3. Data stores

   4. Processes

   \*(Data elements do not have threats because Data is not attacked directly, instead the elements that process Data are attacked)

Every time your threat model indicates that you have a need to mitigate a particular threat, you need to file at least one issue. The first issue goes to the developer to ensure that the developer implements the mitigation called out in the threat model(, and a second issue goes to a tester to ensure that the tester writes tests to verify the mitigation).

If you're not going to follow through on the process and ensure that the threats that you identified are mitigated, then your just wasting your time doing the threat model - it won't actually help you improve the security of your product.
