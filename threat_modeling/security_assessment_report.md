# Security concept / assessment report

A security concept or security assessment is a living document - as you change the design, you need to go back and update your threat model to see if any new threats have arisen since you started.

What goes into the security assessment report:

1. Project information

   1. Overview of project features

   2. List of contacts for questions

   3. Documentation materials - State the reviewed documentation

   4. Relevant requirements - Refer to the relevant requirements/ policies / frameworks / regulations (PCI DSS, ISO 27001, ASVS, etc.)

2. Management summary - Highlight the key findings and recommendations

3. Assessment Methodology - Document the used methodology and metrics

4. Data flow diagram

   1. Data flow diagram element-specific descriptions - A one or two paragraph description of your component and what it does (it helps an outsider to understand your diagram), similarly

   2. Data: List the kind of data that will be processed or stored with data classification

5. Threat Analysis - For each threat that you call out in the threat analysis, you should include the associated mitigation (add issue number of threat or mitigation)

   1. Data flows

   2. External interactors

   3. Data stores

   4. Processes

   \*(Data elements do not have threats because Data is not attacked directly, instead the elements that process Data are attacked)

Every time your threat model indicates that you have a need to mitigate a particular threat, you need to file at least one issue. The first issue goes to the developer to ensure that the developer implements the mitigation called out in the threat model(, and a second issue goes to a tester to ensure that the tester writes tests to verify the mitigation).

If you're not going to follow through on the process and ensure that the threats that you identified are mitigated, then your just wasting your time doing the threat model - it won't actually help you improve the security of your product.
