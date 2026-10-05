# CSSLP Domain 3 Study Guide
## Secure Software Requirements

This study guide focuses on the concepts tested in **CSSLP Domain 3: Secure Software Requirements**, with emphasis on scenario-based questions using words such as:

- **BEST**
- **MOST appropriate**
- **PRIMARY**
- **FIRST**
- **MOST directly**

---

# Domain 3 Overview

Domain 3 is about defining security expectations **before and during development**.

The major areas are:

1. Software security requirements
2. Compliance requirements
3. Data classification
4. Privacy requirements
5. Data access provisioning
6. Misuse and abuse cases
7. Security requirements traceability
8. Third-party vendor security requirements

A simple way to remember Domain 3 is:

```text
Understand the business
        ↓
Identify security requirements
        ↓
Identify compliance obligations
        ↓
Classify the data
        ↓
Define privacy requirements
        ↓
Define access requirements
        ↓
Model misuse and abuse
        ↓
Trace requirements
        ↓
Apply requirements to third parties
```

---

# Fast Exam Recognition Table

| If You See... | Think... |
|---|---|
| What the application must do | **Functional Requirement** |
| Security, reliability, availability, performance | **Non-Functional Requirement** |
| Requirement from law or regulator | **Compliance Requirement** |
| PCI, healthcare, defense, financial rules | **Industry-Specific Requirement** |
| Who is responsible for data | **Data Owner** |
| Who administers or protects data | **Data Custodian** |
| Sensitivity marking | **Data Labeling** |
| Public, confidential, restricted | **Data Classification** |
| Collect only necessary information | **Data Minimization / Collection Scope** |
| Remove identifying information | **Anonymization** |
| Replace identifiers with tokens or aliases | **Pseudonymization** |
| Customer requests deletion | **Right to Erasure / Right to be Forgotten** |
| Data cannot leave a jurisdiction | **Data Residency** |
| Where law applies to processing | **Jurisdiction** |
| New employee receives access | **Provisioning** |
| Existing access must be reviewed periodically | **Reapproval / Recertification** |
| Service identity needs permissions | **Service Account Requirements** |
| How could a legitimate feature be abused? | **Abuse Case** |
| How might an attacker misuse the system? | **Misuse Case** |
| Map requirement to design, code, test | **Traceability Matrix** |
| Vendor must meet defined controls | **Third-Party Security Requirements** |

---

# 3.1 Define Software Security Requirements

Security requirements describe the security behaviors and constraints the system must satisfy.

They should be:

- Clear
- Testable
- Measurable where possible
- Traceable
- Unambiguous
- Relevant to business risk

---

# Functional vs. Non-Functional Requirements

This is one of the most important Domain 3 distinctions.

## Functional Requirement

A functional requirement describes:

> **What the system must do.**

Examples:

- The user shall authenticate using MFA.
- The system shall allow managers to approve payments.
- The application shall lock an account after repeated failed attempts.
- The application shall log administrative actions.

Think:

> **Behavior or capability.**

---

## Non-Functional Requirement

A non-functional requirement describes:

> **How securely, reliably, or under what constraints the system must operate.**

Examples:

- Authentication responses must complete within two seconds.
- Sensitive data must be encrypted at rest.
- The system must support 99.99% availability.
- Audit logs must be retained for seven years.
- The application must recover within a defined recovery time.

Think:

> **Quality, constraint, or operating characteristic.**

---

# Functional vs. Non-Functional Shortcut

```text
Functional
→ What must the system DO?

Non-Functional
→ How must the system BEHAVE or OPERATE?
```

---

# Example

Requirement:

> The system shall require MFA for privileged administrators.

This is primarily a:

> **Functional security requirement**

because it defines required behavior.

Requirement:

> Authentication must remain available during the failure of one authentication node.

This is primarily a:

> **Non-functional availability/resiliency requirement**

---

# Business Requirements

Security requirements should originate from business needs.

Example:

```text
Business Need
    ↓
Protect financial transactions
    ↓
Security Requirement
    ↓
Strong authentication
Transaction authorization
Audit logging
Nonrepudiation
```

### Exam Rule

Security requirements should not be created in isolation from business objectives.

---

# Use Cases

A use case describes how a legitimate actor interacts with the system to achieve a goal.

Example:

```text
Actor: Customer
Goal: Transfer money
```

Possible flow:

```text
Authenticate
    ↓
Select source account
    ↓
Enter destination
    ↓
Enter amount
    ↓
Confirm transaction
```

Security requirements should be derived from important use cases.

---

# User Stories

In Agile environments, requirements may be represented as user stories.

Example:

```text
As an account administrator,
I want privileged actions to require MFA
so that unauthorized administrative access is reduced.
```

Security acceptance criteria can accompany the story.

---

# Requirement Quality

Good requirements should be testable.

Weak:

> The application should use strong security.

Better:

> Administrative users must authenticate using two independent authentication factors.

Weak:

> Sensitive information should be protected.

Better:

> Confidential customer information must be encrypted during transmission using organization-approved cryptography.

---

# Exam Rule

Avoid vague requirements.

A strong requirement should allow someone to later determine:

> **Did the implementation satisfy this requirement or not?**

---

# 3.2 Identify Compliance Requirements

Software requirements may come from external or internal obligations.

Sources may include:

- Regulators
- Laws
- Industry standards
- Contracts
- Organizational standards
- Security frameworks
- Internal policies

---

# Regulatory Requirements

Regulatory requirements originate from regulatory authorities.

Examples can apply to:

- Finance
- Healthcare
- Telecommunications
- Government
- Critical infrastructure

### Exam Rule

When a regulatory requirement applies:

> It must be incorporated into software requirements.

It should not be treated as an optional recommendation.

---

# Legal Requirements

Legal requirements may involve:

- Privacy
- Data retention
- Breach notification
- Intellectual property
- Cross-border processing
- Evidence preservation

### Exam Trap

Do not assume that a technically stronger control can override a legal obligation.

For example:

If law requires data destruction after a certain period, keeping it indefinitely because storage is encrypted is not necessarily compliant.

---

# Industry-Specific Requirements

Different industries may have additional obligations.

Examples include:

- Defense
- Healthcare
- Financial services
- Commercial environments
- Payment card processing

---

# PCI

When a scenario involves:

- Credit card data
- Payment card processing
- Cardholder information

Think:

> **Payment Card Industry requirements**

---

# Company-Wide Requirements

Organizations may define requirements for:

- Approved development tools
- Coding standards
- Security frameworks
- Protocols
- Cryptography
- Authentication
- Logging
- CI/CD controls

These must be incorporated into application requirements where applicable.

---

# Requirement Precedence

When requirements conflict, consider:

```text
Legal / Regulatory
        ↓
Contractual / Industry
        ↓
Organizational Policy
        ↓
Project Preference
```

This is a general exam reasoning aid.

The actual precedence may depend on jurisdiction and contract structure, but project convenience does not override binding obligations.

---

# 3.3 Identify Data Classification Requirements

Data classification determines:

> **How sensitive information is and how it should be protected.**

Classification supports decisions about:

- Access
- Encryption
- Retention
- Transmission
- Storage
- Disposal
- Monitoring

---

# Typical Classification Levels

Organizations may use different names.

Example:

```text
Public
Internal
Confidential
Restricted
```

Higher sensitivity generally requires stronger controls.

---

# Data Owner vs. Data Custodian

Very important distinction.

## Data Owner

The data owner is generally responsible for:

- Determining classification
- Defining access requirements
- Determining appropriate use
- Approving access
- Defining retention requirements

Think:

> **Business accountability for the data.**

---

## Data Custodian

The custodian generally performs operational protection activities.

Examples:

- Backups
- Storage administration
- Access implementation
- Encryption implementation
- System administration

Think:

> **Technical care of the data.**

---

# Owner vs. Custodian Shortcut

```text
Data Owner
→ Decides

Data Custodian
→ Implements / Protects
```

---

# Data Dictionary

A data dictionary defines information about data elements.

It may describe:

- Field names
- Data types
- Meaning
- Format
- Relationships
- Classification
- Ownership

Example:

```text
Field: customer_email
Type: string
Owner: Customer Services
Classification: Confidential
Retention: 5 years
```

---

# Data Labeling

Data labeling identifies the classification or handling requirement.

Examples:

```text
PUBLIC
CONFIDENTIAL
RESTRICTED
```

Labels can support:

- User awareness
- Automated controls
- Data loss prevention
- Access decisions

---

# Classification vs. Labeling

```text
Classification
→ Decide sensitivity

Labeling
→ Mark the data with that classification
```

---

# Structured vs. Unstructured Data

## Structured Data

Has an organized schema.

Examples:

- Database records
- Tables
- CSV fields

---

## Unstructured Data

Does not follow a fixed relational structure.

Examples:

- Documents
- Images
- Emails
- Videos
- Free-form text

### Exam Point

Security requirements must cover both.

Sensitive information does not become less sensitive simply because it is stored in an unstructured form.

---

# Data Lifecycle

Data security requirements should cover the entire lifecycle.

```text
Creation / Collection
        ↓
Use
        ↓
Storage
        ↓
Sharing / Transmission
        ↓
Retention
        ↓
Archival
        ↓
Disposal
```

---

# Exam Rule

Do not protect data only while stored.

Requirements should consider:

> **The entire data lifecycle.**

---

# Data Handling

Handling requirements may define:

- Who may access data
- How data may be transmitted
- Where it may be stored
- Whether it must be encrypted
- How long it may be retained
- How it must be destroyed

---

# PII

**Personally Identifiable Information**

PII is information that identifies or may be linked to an individual.

Examples may include:

- Name
- Address
- Government identifier
- Email address
- Telephone number
- Account identifiers

The exact legal definition depends on jurisdiction.

---

# Publicly Available Information

Public information generally requires less confidentiality protection.

However:

> Public availability does not automatically eliminate integrity or availability requirements.

Example:

A public government website may contain public information but still require strong integrity protection.

---

# 3.4 Identify Privacy Requirements

Privacy focuses on the appropriate collection and use of personal information.

Key topics include:

- Data collection scope
- Anonymization
- User rights
- User preferences
- Retention
- Cross-border processing

---

# Data Minimization

A key privacy principle is:

> **Collect only the information necessary for the defined purpose.**

Example:

If an application only needs an email address to deliver a newsletter, requesting:

- Passport number
- Date of birth
- Home address

may be unnecessary.

---

# Exam Clue

If the question asks:

> What BEST reduces privacy exposure?

A strong answer may be:

> **Do not collect unnecessary personal data in the first place.**

---

# Purpose Limitation

Information should be collected and used for defined legitimate purposes.

Example:

A company collects an email address for order delivery notifications.

Using it later for unrelated marketing may require additional consent or legal basis.

---

# Anonymization

Anonymization attempts to remove the ability to identify a person.

Properly anonymized data should not reasonably be reversible to the individual.

Think:

> **Identity removed.**

---

# Pseudonymization

Pseudonymization replaces identifying information with another identifier.

Example:

```text
Alice Smith
      ↓
User-8F2C1
```

A separate mapping may still allow re-identification.

Therefore:

> Pseudonymized data may still be personal data.

---

# Anonymization vs. Pseudonymization

```text
Anonymization
→ Identity cannot reasonably be restored

Pseudonymization
→ Identity replaced, but may be restored using additional information
```

---

# Exam Trap

Do not assume pseudonymization equals anonymization.

If the mapping still exists:

> Re-identification may still be possible.

---

# User Rights

Depending on applicable law, users may have rights concerning their personal information.

Possible rights include:

- Access
- Correction
- Deletion
- Restriction
- Portability
- Objection

---

# Right to be Forgotten

A user may have a legal right in some circumstances to request deletion of personal data.

However:

> This is not always absolute.

Data may need to be retained due to:

- Legal obligations
- Regulatory requirements
- Fraud prevention
- Contractual obligations
- Litigation holds

### Exam Rule

Do not automatically delete data when another binding obligation requires retention.

---

# User Preferences

Applications may need to track preferences such as:

- Marketing consent
- Data sharing preferences
- Cookie choices
- Third-party sharing
- Communication preferences

---

# Terms of Service

Privacy requirements may also be influenced by:

- Terms of service
- Consent language
- Contracts
- User agreements

---

# Data Retention

Retention requirements answer:

```text
What data?
How long?
Where?
Why?
```

Good retention requirements should define all of these.

---

# Retention Exam Trap

Longer retention is not automatically safer.

Keeping unnecessary data creates:

- Privacy risk
- Breach exposure
- Legal exposure
- Storage costs

---

# Cross-Border Requirements

Cross-border data processing introduces issues such as:

- Data residency
- Jurisdiction
- Transfer restrictions
- Multinational processing requirements

---

# Data Residency

Data residency concerns:

> **Where the data is physically or logically stored.**

Example:

> Customer data must remain in Germany.

---

# Jurisdiction

Jurisdiction concerns:

> **Which legal authority applies to the data or processing.**

Data may be stored in one country while processing or corporate relationships create obligations in another.

---

# Residency vs. Jurisdiction

```text
Data Residency
→ Where is the data?

Jurisdiction
→ Which laws apply?
```

---

# 3.5 Define Data Access Provisioning

Access requirements should define how users and services receive, maintain, and lose access.

Key areas include:

- User provisioning
- Service accounts
- Reapproval
- Deprovisioning
- Access review

---

# User Provisioning

Provisioning is the process of granting access.

A secure process may include:

```text
Request
   ↓
Approval
   ↓
Identity Verification
   ↓
Role Assignment
   ↓
Access Creation
   ↓
Logging
```

---

# Provisioning Should Follow Least Privilege

Users should receive only the permissions required for their role.

Example:

A payroll employee needs payroll processing rights.

They should not automatically receive:

- Database administrator
- Network administrator
- HR administrator

---

# Deprovisioning

When a user leaves or changes roles, inappropriate access should be removed promptly.

Examples:

- Disable account
- Revoke tokens
- Remove group membership
- Revoke privileged roles
- Remove application access

### Exam Clue

Former employee still has access:

> **Deprovisioning failure**

---

# Reapproval / Recertification

Access should not necessarily remain forever simply because it was previously approved.

Periodic reapproval asks:

> **Does this person still need this access?**

---

# Exam Clue

A user accumulated permissions over many years through role changes.

Best control:

> **Periodic access review / recertification**

---

# Service Accounts

Service accounts are identities used by applications or services.

Requirements should define:

- Purpose
- Owner
- Required permissions
- Credential management
- Rotation
- Monitoring
- Lifecycle
- Deactivation

---

# Service Account Exam Trap

Do not assume service accounts should have:

> Administrator access for convenience.

Apply:

> **Least privilege**

---

# Human vs. Service Account

Human account:

> Associated with an individual.

Service account:

> Associated with an application, process, workload, or service.

Both still require:

- Ownership
- Access control
- Monitoring
- Lifecycle management

---

# 3.6 Develop Misuse and Abuse Cases

Normal use cases describe:

> How legitimate users achieve intended goals.

Misuse and abuse cases describe:

> How functionality may be intentionally misused.

---

# Misuse Case

A misuse case examines how an attacker or unauthorized actor may misuse the system.

Example:

Normal function:

> Password reset.

Misuse:

> Attacker abuses password reset to take over another user's account.

---

# Abuse Case

An abuse case often examines how legitimate functionality may be used in a harmful or unintended way.

Example:

Normal function:

> Search customer records.

Abuse:

> Employee performs thousands of searches to collect customer information.

---

# Exam Point

The exact distinction between misuse and abuse terminology can vary.

For CSSLP reasoning, focus on:

> **Thinking like an attacker or malicious user during requirements analysis.**

---

# Misuse / Abuse Flow

```text
Legitimate Feature
       ↓
How could it be abused?
       ↓
Security Weakness
       ↓
Mitigating Control
       ↓
Security Requirement
```

---

# Example

Feature:

> Users can upload images.

Possible misuse:

> User uploads executable content disguised as an image.

Mitigating requirements:

- Validate file type
- Limit size
- Store outside executable paths
- Scan content
- Rename uploaded files
- Restrict permissions

---

# Mitigating Control Identification

The goal is not merely to document abuse.

The process should identify:

> **Controls that reduce the identified threat.**

---

# Exam Rule

If a misuse case identifies a threat, the next important step is usually:

> **Define mitigating security requirements or controls.**

---

# 3.7 Develop Security Requirements Traceability Matrix

A **Security Requirements Traceability Matrix** helps demonstrate that security requirements are carried through the lifecycle.

Think:

```text
Requirement
    ↓
Design
    ↓
Implementation
    ↓
Test Case
    ↓
Test Result
```

---

# Example Traceability Matrix

| Requirement | Design | Implementation | Test |
|---|---|---|---|
| SEC-001 MFA required for administrators | IAM Design 4.2 | Auth Module | TEST-101 |
| SEC-002 Audit privileged changes | Logging Design 3.4 | Audit Service | TEST-155 |
| SEC-003 Encrypt customer records | Data Design 6.1 | Encryption Layer | TEST-201 |

---

# Why Traceability Matters

Traceability helps answer:

- Was every security requirement implemented?
- Was every requirement tested?
- Which design component satisfies the requirement?
- What must be retested if a requirement changes?
- Can compliance obligations be demonstrated?

---

# Forward Traceability

Starts with a requirement and follows it into implementation/testing.

```text
Requirement
    ↓
Design
    ↓
Code
    ↓
Test
```

Question answered:

> **Was this requirement implemented and tested?**

---

# Backward Traceability

Starts with implementation or a feature and traces back to its requirement.

```text
Feature
   ↓
Requirement
   ↓
Business / Compliance Need
```

Question answered:

> **Why does this functionality exist?**

---

# Traceability Exam Clue

If a question asks:

> How can an organization prove that every security requirement was implemented and tested?

Think:

> **Security Requirements Traceability Matrix**

---

# Traceability vs. Testing

Testing proves:

> A control behaves correctly.

Traceability proves:

> The requirement is connected through the lifecycle.

They are related but not the same.

---

# 3.8 Define Third-Party Vendor Security Requirements

Third parties can introduce significant software risk.

Examples include:

- SaaS providers
- Cloud providers
- Payment processors
- Libraries
- Outsourced developers
- Managed services
- API providers

Security requirements should be defined before or during acquisition.

---

# Vendor Requirements May Include

- Authentication requirements
- Encryption
- Data handling
- Privacy
- Logging
- Incident notification
- Vulnerability management
- Patch timelines
- Secure development practices
- Audit rights
- Data deletion
- Availability
- Business continuity
- Compliance certifications

---

# Third-Party Data Handling

If a vendor processes sensitive information, requirements should define:

```text
What data?
        ↓
Why?
        ↓
Where?
        ↓
Who may access it?
        ↓
How is it protected?
        ↓
How long is it retained?
        ↓
How is it deleted?
```

---

# Shared Responsibility

Using a third party does not automatically eliminate the customer's security responsibilities.

Example:

```text
Cloud Provider
→ Secures underlying infrastructure

Customer
→ May still be responsible for configuration, identity, data, and application controls
```

### Exam Rule

Outsourcing a service does not necessarily mean:

> **Outsourcing accountability.**

---

# Contractual Security Requirements

Important security requirements should be enforceable where appropriate.

Contracts may specify:

- Notification timelines
- Security standards
- Audit rights
- Data ownership
- Data return
- Data deletion
- Service levels
- Security responsibilities

---

# Incident Notification

Vendor requirements should define:

> How quickly must the organization be informed of a security incident?

Weak:

> Vendor will notify us when convenient.

Better:

> Vendor must notify the organization within the contractually defined period following discovery of a relevant security incident.

---

# Right to Audit

A right-to-audit provision may allow the organization to verify vendor compliance.

Possible mechanisms include:

- Direct audits
- Independent assessments
- Evidence review
- Compliance reports

---

# Vendor Exit Requirements

Security requirements should also cover termination.

Questions include:

- How will data be returned?
- How will copies be destroyed?
- When will vendor access be removed?
- How will credentials be revoked?
- What happens to backups?
- How is deletion verified?

---

# High-Value Exam Distinctions

## Functional vs. Non-Functional

```text
Functional
→ What does the system do?

Non-Functional
→ Under what security / quality constraints does it operate?
```

---

## Data Owner vs. Data Custodian

```text
Owner
→ Decides classification and access

Custodian
→ Implements protection
```

---

## Classification vs. Labeling

```text
Classification
→ Determine sensitivity

Labeling
→ Mark the data accordingly
```

---

## Anonymization vs. Pseudonymization

```text
Anonymization
→ Re-identification should not reasonably be possible

Pseudonymization
→ Identifiers replaced, but re-identification may remain possible
```

---

## Residency vs. Jurisdiction

```text
Residency
→ Where is data stored?

Jurisdiction
→ Which law applies?
```

---

## Provisioning vs. Reapproval

```text
Provisioning
→ Grant access

Reapproval
→ Confirm continued need for access
```

---

## Use Case vs. Misuse Case

```text
Use Case
→ Intended behavior

Misuse Case
→ Malicious or unintended behavior
```

---

## Testing vs. Traceability

```text
Testing
→ Does the control work?

Traceability
→ Was the requirement implemented and tested throughout the lifecycle?
```

---

# Common CSSLP Domain 3 Exam Traps

## Trap 1 — Vague Requirements

Weak:

> Protect customer data.

Better:

> Customer data classified as Confidential must be encrypted at rest using organization-approved cryptography.

Security requirements should be:

> **Specific and testable.**

---

## Trap 2 — Technical Requirements Before Business Requirements

Wrong approach:

> Select encryption algorithms first.

Better approach:

```text
Understand business need
        ↓
Determine data sensitivity
        ↓
Identify compliance requirements
        ↓
Define security requirement
        ↓
Choose implementation
```

---

## Trap 3 — Data Owner vs. System Administrator

Wrong:

> Administrator decides business classification because they manage the database.

Better:

> **Data owner determines classification.**

The custodian implements the appropriate protections.

---

## Trap 4 — Collect Everything

Wrong:

> Collect additional personal data in case it becomes useful.

Better:

> **Collect only necessary information.**

This reduces privacy and security exposure.

---

## Trap 5 — Pseudonymization Means Anonymous

Wrong:

> Replace names with IDs, therefore the data is anonymous.

Better:

> If identity can be restored using another mapping, it is **pseudonymized**.

---

## Trap 6 — Access Never Reviewed

Wrong:

> A manager approved access three years ago, so the user can keep it indefinitely.

Better:

> Conduct periodic **reapproval / recertification**.

---

## Trap 7 — Shared Service Administrator Account

Wrong:

> Give every service account full administrative access to simplify management.

Better:

> Define ownership and apply **least privilege**.

---

## Trap 8 — Only Model Legitimate Use

Wrong:

> Requirements only need normal user scenarios.

Better:

> Develop **misuse and abuse cases** to identify attacker behavior.

---

## Trap 9 — Requirements Without Traceability

Wrong:

> Security requirements are documented, therefore the project is compliant.

Better:

> Requirements must be traceable to design, implementation, and testing.

---

## Trap 10 — Vendor Is Responsible for Everything

Wrong:

> The vendor hosts the application, so our organization has no remaining security responsibility.

Better:

> Define responsibilities explicitly and manage third-party risk.

---

# Scenario Recognition Examples

## Scenario 1

A requirement says:

> The application must support 50,000 concurrent users without losing availability.

Think:

> **Non-functional requirement**

---

## Scenario 2

A requirement says:

> Users must authenticate with MFA before approving a payment.

Think:

> **Functional security requirement**

---

## Scenario 3

A database administrator is asked to decide whether customer information should be classified Confidential or Restricted.

Best response:

> The **data owner** should determine the classification.

The administrator acts primarily as the custodian.

---

## Scenario 4

An application collects birth dates, passport numbers, phone numbers, addresses, and national identifiers even though only an email address is needed.

Think:

> **Data minimization / collection scope problem**

---

## Scenario 5

Customer names are replaced with randomly generated IDs, but a separate protected database maps each ID to the customer's name.

Think:

> **Pseudonymization**

Not full anonymization.

---

## Scenario 6

A user moved from Finance to Marketing two years ago but still has Finance system access.

Think:

> **Reapproval / access recertification / deprovisioning problem**

---

## Scenario 7

A normal requirement says:

> Customers may reset their passwords.

Security analysts ask:

> Could an attacker use password reset to take over another user's account?

Think:

> **Misuse / abuse case analysis**

---

## Scenario 8

Auditors ask how a regulatory requirement was implemented and tested.

The organization can show:

```text
Regulation
   ↓
Security Requirement
   ↓
Design Component
   ↓
Implementation
   ↓
Test Case
```

Think:

> **Security Requirements Traceability Matrix**

---

## Scenario 9

A SaaS vendor stores customer information overseas.

The organization needs to determine whether that location is legally permitted.

Think:

> **Cross-border processing / residency / jurisdiction**

---

## Scenario 10

A vendor contract describes availability but says nothing about breach notification or data deletion at termination.

Think:

> **Incomplete third-party security requirements**

---

# Requirement Development Workflow

A useful exam-oriented workflow is:

```text
Business Requirements
        ↓
Identify Assets and Data
        ↓
Identify Compliance Obligations
        ↓
Classify Data
        ↓
Identify Privacy Requirements
        ↓
Define Access Requirements
        ↓
Develop Misuse / Abuse Cases
        ↓
Define Mitigating Security Requirements
        ↓
Create Traceability
        ↓
Validate Third-Party Requirements
```

---

# Requirements Should Be Testable

A requirement is much stronger if it has measurable acceptance criteria.

Weak:

```text
Passwords must be secure.
```

Better:

```text
Privileged users must authenticate using MFA.
```

Weak:

```text
Logs should be kept for a long time.
```

Better:

```text
Security audit logs must be retained for 365 days.
```

---

# Requirement vs. Control

A requirement defines:

> **What must be achieved.**

A control implements:

> **How the requirement is satisfied.**

Example:

Requirement:

```text
Only authorized administrators may modify customer records.
```

Possible controls:

```text
RBAC
MFA
Audit logging
Privileged access management
```

### Exam Rule

Do not jump directly to a control before understanding the requirement.

---

# Requirement vs. Design

Requirement:

> What must be achieved?

Design:

> How will the system achieve it?

Example:

Requirement:

```text
Sensitive data must be protected in transit.
```

Design decision:

```text
Use TLS with organization-approved configuration.
```

---

# Privacy by Design

Privacy requirements should be incorporated early rather than added after deployment.

Think:

```text
Requirements
    ↓
Privacy
    ↓
Architecture
    ↓
Implementation
    ↓
Testing
```

Examples:

- Data minimization
- Purpose limitation
- Retention requirements
- Access restrictions
- User rights
- Secure deletion

---

# Security by Design and Requirements

Security requirements should be identified early because late changes are generally:

- More expensive
- Harder to implement
- More disruptive
- More likely to be incomplete

---

# Fast Memory Sheet

```text
FUNCTIONAL REQUIREMENT
→ What must the system do?

NON-FUNCTIONAL REQUIREMENT
→ How must the system operate?

COMPLIANCE
→ What external/internal obligations apply?

DATA OWNER
→ Decides classification and access.

DATA CUSTODIAN
→ Implements protection.

CLASSIFICATION
→ How sensitive is the data?

LABELING
→ Mark the data with the classification.

DATA LIFECYCLE
→ Creation through disposal.

DATA MINIMIZATION
→ Collect only what is needed.

ANONYMIZATION
→ Identity cannot reasonably be restored.

PSEUDONYMIZATION
→ Identity replaced but potentially recoverable.

RETENTION
→ How long should data be kept?

RESIDENCY
→ Where is the data?

JURISDICTION
→ Which laws apply?

PROVISIONING
→ Grant access.

REAPPROVAL
→ Confirm access is still required.

SERVICE ACCOUNT
→ Non-human identity requiring ownership and least privilege.

MISUSE / ABUSE CASE
→ How could functionality be attacked or abused?

MITIGATING CONTROL
→ Reduce identified misuse risk.

TRACEABILITY MATRIX
→ Requirement → Design → Implementation → Test.

THIRD-PARTY REQUIREMENT
→ Security obligations imposed on vendors.
```

---

# Hardest Domain 3 Distinctions to Memorize

| Question Is Asking... | Likely Concept |
|---|---|
| What must the application do? | **Functional Requirement** |
| Under what security constraint must it operate? | **Non-Functional Requirement** |
| Who determines data sensitivity? | **Data Owner** |
| Who applies technical safeguards? | **Data Custodian** |
| How sensitive is the information? | **Classification** |
| How is sensitivity communicated? | **Labeling** |
| Can identity be reconstructed? | **Pseudonymization** |
| Has identity been irreversibly removed? | **Anonymization** |
| Where is information stored? | **Residency** |
| Which law applies? | **Jurisdiction** |
| Does the user still need access? | **Reapproval** |
| How could intended functionality be abused? | **Misuse / Abuse Case** |
| Was each requirement implemented and tested? | **Traceability Matrix** |
| What must a vendor contractually protect? | **Third-Party Security Requirement** |

---

# CSSLP Domain 3 Exam Strategy

Domain 3 questions often test whether you understand the difference between:

```text
Business Need
     ↓
Requirement
     ↓
Design
     ↓
Control
     ↓
Test
```

Do not confuse these stages.

Example:

Business need:

> Protect customer financial information.

Requirement:

> Only authorized personnel may access customer financial records.

Design:

> Implement role-based access control.

Control:

> IAM roles and authorization checks.

Testing:

> Verify unauthorized users cannot access financial records.

---

# FIRST / BEST Exam Logic

If a question asks what should be done **FIRST**, the answer is often closer to:

> Understand requirements, data, business context, or compliance obligations.

Rather than immediately choosing:

- A product
- A protocol
- An encryption algorithm
- A security tool

---

# Example

Question:

> A company is building an application that will process sensitive medical data. What should the security architect do FIRST?

Possible answers:

A. Select an encryption library  
B. Install a web application firewall  
C. Identify applicable regulatory and privacy requirements  
D. Perform penetration testing  

Best answer:

> **C**

Because requirements and obligations must be understood before controls are selected.

---

# BEST Exam Logic

If two answers seem technically correct, choose the one that:

1. Addresses the requirement directly
2. Aligns with business risk
3. Satisfies compliance obligations
4. Is testable
5. Can be traced through the lifecycle

---

# Domain 3 Master Sequence

Memorize this sequence:

```text
Business
   ↓
Compliance
   ↓
Data
   ↓
Privacy
   ↓
Access
   ↓
Misuse
   ↓
Requirements
   ↓
Traceability
   ↓
Third Parties
```

---

# Final Exam Rules

### Rule 1

> Security requirements should be defined before implementation decisions.

### Rule 2

> Requirements should be clear, measurable, and testable.

### Rule 3

> Data owners determine classification; custodians implement protection.

### Rule 4

> Protect data across its entire lifecycle.

### Rule 5

> Collect only the personal information that is necessary.

### Rule 6

> Pseudonymized data is not automatically anonymous.

### Rule 7

> Access must be provisioned, reviewed, and eventually removed.

### Rule 8

> Misuse and abuse cases help identify requirements that normal use cases miss.

### Rule 9

> Security requirements must be traceable to implementation and testing.

### Rule 10

> Third-party vendors must have explicit security requirements rather than assumed responsibilities.

### Rule 11

> When legal, regulatory, contractual, and organizational requirements apply, identify them early.

### Rule 12

> If a question asks FIRST, understand requirements and risk before selecting a technical control.

---

## Disclaimer

This is an independent CSSLP study guide and is not an official ISC2 publication or a collection of official exam questions.
