# Web application checklist

Resource:

- https://zeltser.com/security-architecture-cheat-sheet/

Questions about the initial design and review of a complex Internet application's security architecture.

Depending on the web application / service, questions can be added or removed.

## Business Requirements

### Business Model

| Question                                                                              | Answer | Interview notes |
| ------------------------------------------------------------------------------------- | ------ | --------------- |
| What is the application's primary business purpose?                                   |        |                 |
| What are the planned business milestones for developing or improving the application? |        |                 |
| What key benefits does the application offer its users?                               |        |                 |
| What business continuity provisions have been defined for the application?            |        |                 |
| What geographic areas does the application service?                                   |        |                 |

### Data Essentials

| Question                                                                           | Answer | Interview notes |
| ---------------------------------------------------------------------------------- | ------ | --------------- |
| What data does the application receive, produce, and process?                      |        |                 |
| How can the data be classified into categories according to its sensitivity?       |        |                 |
| How might an attacker benefit from capturing or modifying the data?                |        |                 |
| What data backup and retention requirements have been defined for the application? |        |                 |

### End-Users

| Question                                            | Answer | Interview notes |
| --------------------------------------------------- | ------ | --------------- |
| Who are the application's end-users?                |        |                 |
| How do the end-users interact with the application? |        |                 |
| What security expectations do the end-users have?   |        |                 |

### Partners

| Question                                                                                  | Answer | Interview notes |
| ----------------------------------------------------------------------------------------- | ------ | --------------- |
| Which third-parties supply data to the application?                                       |        |                 |
| Which third-parties receive data from the applications?                                   |        |                 |
| Which third-parties process the application's data?                                       |        |                 |
| What mechanisms are used to share data with third-parties besides the application itself? |        |                 |
| What security requirements do the partners impose?                                        |        |                 |

### Administrators

| Question                                                     | Answer | Interview notes |
| ------------------------------------------------------------ | ------ | --------------- |
| Who has administrative capabilities in the application?      |        |                 |
| What administrative capabilities does the application offer? |        |                 |

### Regulations

| Question                                                         | Answer | Interview notes |
| ---------------------------------------------------------------- | ------ | --------------- |
| In what industries does the application operate?                 |        |                 |
| What security-related regulations apply?                         |        |                 |
| What auditing and compliance regulations apply?                  |        |                 |
| What policies, standards, or regulations are you compliant with? |        |                 |

## Infrastructure Requirements

### Network

| Question                                                                                      | Answer | Interview notes |
| --------------------------------------------------------------------------------------------- | ------ | --------------- |
| What details regarding routing, switching, firewalling, and load-balancing have been defined? |        |                 |
| What network design supports the application?                                                 |        |                 |
| What network performance requirements exist?                                                  |        |                 |

### Systems

| Question                                                                             | Answer | Interview notes |
| ------------------------------------------------------------------------------------ | ------ | --------------- |
| What operating systems support the application?                                      |        |                 |
| What hardware requirements have been defined?                                        |        |                 |
| What details regarding required OS components and lock-down needs have been defined? |        |                 |

### Infrastructure Monitoring

| Question                                                                              | Answer | Interview notes |
| ------------------------------------------------------------------------------------- | ------ | --------------- |
| What network and system performance monitoring requirements have been defined?        |        |                 |
| What mechanisms exist to detect malicious code or compromised application components? |        |                 |
| What network and system security monitoring requirements have been defined?           |        |                 |

### Virtualization and Externalization

| Question                                                                            | Answer | Interview notes |
| ----------------------------------------------------------------------------------- | ------ | --------------- |
| What aspects of the application lend themselves to virtualization?                  |        |                 |
| What virtualization requirements have been defined for the application?             |        |                 |
| What aspects of the product may or may not be hosted via the cloud computing model? |        |                 |

## Application Requirements

### Environment

| Question                                                                                  | Answer | Interview notes |
| ----------------------------------------------------------------------------------------- | ------ | --------------- |
| What frameworks and programming languages have been used to create the application?       |        |                 |
| What process, code, or infrastructure dependencies have been defined for the application? |        |                 |
| Do you hanlde 3rd party dependency risk?                                                  |        |                 |
| Do you prevent build of software it it's affected by vulnerabilities in dependencies?     |        |                 |
| What databases and application servers support the application?                           |        |                 |
| Do you harden configuration for key components of your technology stacks?                 |        |                 |

### Data Processing

| Question                                                                                         | Answer | Interview notes |
| ------------------------------------------------------------------------------------------------ | ------ | --------------- |
| What data entry paths does the application support?                                              |        |                 |
| What data output paths does the application support?                                             |        |                 |
| How does data flow across the application's internal components?                                 |        |                 |
| What data input validation requirements have been defined?                                       |        |                 |
| What data does the application store and how, including sensitivity levels?                      |        |                 |
| What data is or may need to be encrypted and what key management requirements have been defined? |        |                 |
| What capabilities exist to detect the leakage of sensitive data?                                 |        |                 |

### Access

| Question                                                                    | Answer | Interview notes |
| --------------------------------------------------------------------------- | ------ | --------------- |
| What user privilege levels does the application support?                    |        |                 |
| What user identification and authentication requirements have been defined? |        |                 |
| What user authorization requirements have been defined?                     |        |                 |
| What session management requirements have been defined?                     |        |                 |
| What access requirements have been defined for URI and Service calls?       |        |                 |
| What user access restrictions have been defined?                            |        |                 |
| How are user identities maintained throughout transaction calls?            |        |                 |

### Application Monitoring

| Question                                                                    | Answer | Interview notes |
| --------------------------------------------------------------------------- | ------ | --------------- |
| What application auditing requirements have been defined?                   |        |                 |
| What application performance monitoring requirements have been defined?     |        |                 |
| What application security monitoring requirements have been defined?        |        |                 |
| What application error handling and logging requirements have been defined? |        |                 |
| How are audit and debug logs accessed, stored, and secured?                 |        |                 |

### Application Design

| Question                                                                                          | Answer | Interview notes |
| ------------------------------------------------------------------------------------------------- | ------ | --------------- |
| What application design review practices have been defined and executed?                          |        |                 |
| How is intermediate or in-process data stored in the application components' memory and in cache? |        |                 |
| What staging, testing, and Quality Assurance requirements have been defined?                      |        |                 |
| Do you use shared security services?                                                              |        |                 |

## Security Program Requirements

### Application Risk Profile

| Question                                                                    | Answer | Interview notes |
| --------------------------------------------------------------------------- | ------ | --------------- |
| Does an application risk profile exist?                                     |        |                 |
| Do you regularly review and update the application risk profile?            |        |                 |
| Do you identify and manage architectural design flaws with threat modeling? |        |                 |
| Do you regularly review and update the threat model?                        |        |                 |

### Operations

| Question                                                                                             | Answer | Interview notes |
| ---------------------------------------------------------------------------------------------------- | ------ | --------------- |
| What is the process for identifying and addressing vulnerabilities in the application?               |        |                 |
| What is the process for identifying and addressing vulnerabilities in network and system components? |        |                 |
| What access to system and network administrators have to the application's sensitive data?           |        |                 |
| What security incident requirements have been defined?                                               |        |                 |
| How do administrators access production infrastructure to manage it?                                 |        |                 |
| What physical controls restrict access to the application's components and data?                     |        |                 |
| What is the process for granting access to the environment hosting the application?                  |        |                 |

### Change Management

| Question                                                                   | Answer | Interview notes |
| -------------------------------------------------------------------------- | ------ | --------------- |
| How are changes to the code controlled?                                    |        |                 |
| How are changes to the infrastructure controlled?                          |        |                 |
| How is code deployed to production?                                        |        |                 |
| What mechanisms exist to detect violations of change management practices? |        |                 |
| Do you use repeatable deployment processes?                                |        |                 |

### Software Development

| Question                                                                                    | Answer | Interview notes |
| ------------------------------------------------------------------------------------------- | ------ | --------------- |
| What data is available to developers for testing?                                           |        |                 |
| How do developers assist with troubleshooting and debugging the application?                |        |                 |
| What requirements have been defined for controlling access to the applications source code? |        |                 |
| What secure coding processes have been established?                                         |        |                 |
| Is your full build process formally described?                                              |        |                 |
| Do you enforce automated security checks in your build processes?                           |        |                 |
| Do you scan applications with automated security testing tools?                             |        |                 |
| Do you integrate automated security testing into the CI / CD pipeline?                      |        |                 |
| Do you perform penetration testing for your application at regular intervals?               |        |                 |

### Corporate

| Question                                                                                             | Answer | Interview notes |
| ---------------------------------------------------------------------------------------------------- | ------ | --------------- |
| What corporate security program requirements have been defined?                                      |        |                 |
| What security training do developers and administrators undergo?                                     |        |                 |
| Which personnel oversees security processes and requirements related to the application?             |        |                 |
| What employee initiation and termination procedures have been defined?                               |        |                 |
| What application requirements impose the need to enforce the principle of separation of duties?      |        |                 |
| What controls exist to protect a compromised in the corporate environment from affecting production? |        |                 |
| What security governance requirements have been defined?                                             |        |                 |
