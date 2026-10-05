# CSSLP Domain 8 Study Guide
## Secure Software Supply Chain

This study guide focuses on **CSSLP Domain 8: Secure Software Supply Chain**.

Domain 8 covers security risks introduced by software, components, suppliers, repositories, build systems, services, and contractual relationships that exist outside or upstream of your own development team.

The major areas are:

1. Software supply chain risk management
2. Third-party software security analysis
3. Pedigree and provenance verification
4. Supplier security requirements during acquisition
5. Contractual security requirements

---

# Domain 8 Mental Model

Think of Domain 8 as:

```text
Select
  ↓
Assess
  ↓
Inventory
  ↓
Verify Origin
  ↓
Secure Transfer
  ↓
Integrate
  ↓
Monitor
  ↓
Contract
  ↓
Audit
  ↓
Respond to Changes
```

A simple exam question to ask yourself is:

> **Do we know what software we depend on, where it came from, whether we can trust it, and what the supplier is obligated to do?**

---

# Fast Exam Recognition Table

| If You See... | Think... |
|---|---|
| Evaluate risk before selecting a library | **Supply Chain Risk Management** |
| List all third-party components | **SBOM** |
| New vulnerability discovered in dependency | **Continuous Component Monitoring** |
| Vendor has security certification | **Third-Party Security Evidence** |
| Review supplier's assessment report | **Third-Party Software Analysis** |
| Determine where software originated | **Provenance** |
| Determine historical quality/background of component | **Pedigree** |
| Verify downloaded package hasn't changed | **Hash / Digital Signature** |
| Maintain evidence of who handled an artifact | **Chain of Custody** |
| Protect source repository | **Repository Security** |
| Protect compiler/build server | **Build Environment Security** |
| Vendor must allow inspection | **Right to Audit** |
| Require vendor to notify of vulnerabilities | **Supplier Security Requirement** |
| Require incident notification | **Contractual Incident Requirement** |
| Open-source project no longer maintained | **Support / Maintenance Risk** |
| Commercial vendor vs. community support | **Maintenance Structure** |
| Contract defines who performs testing | **Shared Responsibility** |
| Vendor logs must feed organization SIEM | **Log Integration Requirement** |
| Who owns source code? | **Intellectual Property** |
| Source held by trusted third party | **Code Escrow** |
| Who pays for damages? | **Liability** |
| Vendor promises product characteristics | **Warranty** |
| Software usage terms | **EULA** |
| Uptime/support commitment | **SLA** |

---

# 8.1 Implement Software Supply Chain Risk Management

Software rarely consists entirely of code written internally.

Modern applications may depend on:

- Open-source libraries
- Commercial libraries
- Frameworks
- Packages
- Containers
- Operating systems
- APIs
- SaaS providers
- Cloud platforms
- Build tools
- Development tools
- Third-party services

Each dependency introduces:

> **Supply-chain risk**

---

# What Is the Software Supply Chain?

A simplified software supply chain may look like:

```text
Developer
   ↓
Source Repository
   ↓
Dependencies
   ↓
Build System
   ↓
Artifact Repository
   ↓
Deployment Pipeline
   ↓
Production
```

Any compromised stage may affect the final software.

---

# Supply Chain Security Principle

Do not ask only:

> Is our source code secure?

Also ask:

```text
Are our dependencies secure?

Is the repository secure?

Is the build environment secure?

Are artifacts authentic?

Are suppliers trustworthy?

Can we detect upstream changes?
```

---

# Component Identification and Selection

Before selecting a component, evaluate factors such as:

- Security history
- Maintenance status
- Vendor/community reputation
- Known vulnerabilities
- Update frequency
- Support lifecycle
- Licensing
- Required privileges
- Dependencies
- Provenance

---

# Selection Exam Rule

The easiest component to integrate is not automatically:

> The most appropriate component.

Selection should consider:

> **Security risk throughout the component lifecycle.**

---

# Component Risk Assessment

Possible questions include:

```text
What does this component do?

What privileges does it require?

What data can it access?

Who maintains it?

What happens if it is compromised?

Can we update or replace it?

Does it introduce transitive dependencies?
```

---

# Risk Treatment

After assessing component risk, possible responses include:

```text
Avoid
Mitigate
Transfer
Accept
```

---

# Avoid

Do not use the risky component.

Example:

> Select a different library with active maintenance and fewer known vulnerabilities.

---

# Mitigate

Reduce the risk.

Examples:

- Sandbox component
- Restrict privileges
- Patch
- Add monitoring
- Remove unused functionality

---

# Transfer

Shift some contractual or financial consequences.

Examples:

- Insurance
- Vendor liability
- Contractual obligations

Remember:

> Risk transfer rarely eliminates your own security responsibilities.

---

# Accept

Appropriate authority accepts residual risk.

### Exam Rule

Developers should not casually accept major third-party risk simply because:

> Replacing the dependency would be difficult.

---

# Software Bill of Materials — SBOM

An **SBOM** is an inventory of software components contained in a product.

It can include:

- Component name
- Version
- Supplier
- Dependency relationships
- Licensing information
- Package identifiers

Conceptually:

```text
Application
   ↓
Library A v2.1
Library B v4.6
Framework C v7.0
Package D v1.2
```

---

# Why SBOM Matters

Imagine a critical vulnerability is announced in:

```text
Library B v4.6
```

Without an inventory:

> Which applications are affected?

With an SBOM:

> Search the component inventory and identify affected applications quickly.

---

# SBOM Memory Phrase

> **What software is inside our software?**

---

# SBOM vs. SCA

This distinction is important.

## SBOM

An inventory.

Think:

> **What components do we have?**

## Software Composition Analysis — SCA

Analyzes third-party components for issues such as:

- Known vulnerabilities
- Versions
- Licenses
- Dependencies

Think:

> **What problems exist in those components?**

---

# Shortcut

```text
SBOM
→ Inventory

SCA
→ Analyze the inventory/components
```

---

# SBOM Is Not a Security Guarantee

Having an SBOM does not mean:

> All components are secure.

It improves:

- Visibility
- Response
- Traceability

You still need to:

- Assess
- Monitor
- Patch
- Replace
- Mitigate

---

# Transitive Dependencies

An application may directly depend on Library A.

But Library A depends on B, and B depends on C.

```text
Application
    ↓
Library A
    ↓
Library B
    ↓
Library C
```

A vulnerability in C may still compromise:

> Your application.

These are:

> **Transitive dependencies**

---

# Exam Trap

Wrong:

> We only need to track libraries our developers directly selected.

Better:

> Track direct and relevant transitive dependencies.

---

# Component Monitoring

Supply-chain risk management is continuous.

After selecting a component, monitor for:

- New vulnerabilities
- New releases
- End-of-life announcements
- Ownership changes
- Compromise reports
- Licensing changes
- Maintenance abandonment

---

# Exam Rule

A component that was secure when selected may:

> Become vulnerable later.

Therefore component evaluation is not a one-time activity.

---

# End-of-Life Components

An unsupported dependency creates risk because:

- Vulnerabilities may remain unfixed
- Security updates stop
- Compatibility problems grow

Think:

> **Support lifecycle is part of security risk.**

---

# Component Change Monitoring

A change in a third-party component can affect:

- Security
- Compatibility
- Functionality
- Licensing
- Privacy
- Performance

Therefore updates should be:

- Evaluated
- Tested
- Approved
- Monitored

---

# Supply Chain Risk Frameworks

Organizations may use established frameworks and guidance, including those from:

- ISO
- NIST

The exam is more likely to test:

> Why structured supply-chain risk management matters

than obscure framework details.

---

# 8.2 Analyze Security of Third-Party Software

Third-party software should not automatically be trusted because:

- It is commercial
- It is widely used
- It is open source
- The vendor is large
- It has existed for many years

Instead, gather evidence.

---

# Third-Party Security Evidence

Evidence may include:

- Certifications
- Assessment reports
- Independent audit results
- Vendor documentation
- Penetration-testing summaries
- Security policies
- Vulnerability history

---

# Certification

Certification can provide evidence that:

> An organization or product has been assessed against defined criteria.

However:

> Certification does not guarantee zero vulnerabilities.

---

# Certification Exam Trap

Wrong:

> The vendor is certified, so no additional risk analysis is necessary.

Better:

> Certification is one piece of assurance evidence.

---

# Assessment Reports

Assessment reports may provide information about:

- Controls
- Security maturity
- Compliance
- Identified weaknesses
- Scope
- Exceptions

---

# Cloud Controls Matrix

When using cloud services, structured cloud control assessments may help evaluate:

- Provider responsibilities
- Customer responsibilities
- Control implementation

---

# Assessment Scope Matters

A report may be valid but not cover:

> The service you are actually purchasing.

Always consider:

```text
What was assessed?

When?

Which systems?

Which locations?

Which controls?

What exclusions?
```

---

# Origin

Origin asks:

> **Where did the software come from?**

Examples:

- Official vendor
- Trusted repository
- Unknown mirror
- Developer's personal download
- Community repository

---

# Support

Support asks:

> **Who will maintain this component?**

Consider:

- Commercial support
- Community support
- Internal support
- No support

---

# Community vs. Commercial Support

Neither is automatically more secure.

## Community Support

May offer:

- Transparency
- Many contributors
- Rapid updates

Potential concerns:

- Uncertain response commitments
- Maintainer availability
- No contractual SLA

## Commercial Support

May provide:

- Contractual commitments
- Defined support periods
- Escalation channels

Potential concerns:

- Vendor dependency
- Cost
- Closed development process

---

# Exam Rule

Evaluate:

> **Actual support capability**, not merely the support model label.

---

# Security Track Record

Useful questions:

```text
How quickly are vulnerabilities fixed?

How does the supplier communicate incidents?

Have security problems repeatedly occurred?

Are secure development practices evident?
```

A poor track record may increase supplier risk.

---

# 8.3 Verify Pedigree and Provenance

These concepts are central to Domain 8.

---

# Provenance

Provenance answers:

> **Where did this specific software artifact come from?**

Think:

- Origin
- Source
- Build
- Supplier
- Authenticity

---

# Pedigree

Pedigree is broader historical information about a component.

It may include:

- Development history
- Maintainers
- Previous versions
- Security history
- Quality
- Support history

---

# Shortcut

```text
Provenance
→ Where did THIS artifact come from?

Pedigree
→ What is the component's HISTORY/background?
```

---

# Example

You download:

```text
library-x-4.2.tar.gz
```

Questions about provenance:

```text
Did it come from the official supplier?

Was it built by the authorized pipeline?

Has it been altered?
```

Questions about pedigree:

```text
Who maintains Library X?

How long has it existed?

What is its vulnerability history?

Is it actively maintained?
```

---

# Secure Transfer

Software should be transferred in ways that protect:

- Integrity
- Authenticity
- Confidentiality where required

---

# Chain of Custody

Chain of custody records:

> Who possessed or controlled an artifact and when.

This concept is useful when artifacts move between:

- Suppliers
- Build systems
- Repositories
- Organizations

---

# Chain of Custody vs. Chain of Trust

Useful distinction:

```text
Chain of Custody
→ Who handled the artifact?

Chain of Trust
→ Why do we trust each verification step?
```

---

# Artifact Integrity

Cryptographic hashing helps detect:

> Modification.

Example:

```text
Artifact
   ↓
SHA-256
   ↓
Expected Hash
```

---

# Artifact Authenticity

Digital signatures provide stronger evidence that:

- Artifact was not changed
- Artifact was signed by holder of expected signing key

---

# Hash vs. Digital Signature

```text
Hash
→ Integrity

Digital Signature
→ Integrity + Authenticity
```

---

# Important Supply Chain Attack

If attackers compromise a download server and replace:

```text
Software
+
Published Hash
```

a plain hash may not help.

A signature verified using an independently trusted public key provides:

> Stronger authenticity assurance.

---

# Signed Components

Before accepting a signed component:

1. Verify signature
2. Verify signing identity
3. Verify trust in signing key/certificate
4. Verify artifact is expected version

A technically valid signature from:

> The wrong signer

does not establish trust.

---

# Repository Security

Source-code and artifact repositories are highly sensitive.

A compromised repository may allow attackers to:

- Modify source
- Replace dependencies
- Steal secrets
- Inject malicious code
- Alter releases

---

# Repository Controls

Possible controls:

- MFA
- Least privilege
- Branch protection
- Approval requirements
- Audit logging
- Signed commits/tags
- Protected release branches
- Secrets scanning

---

# Branch Protection

Critical branches may require:

```text
Pull Request
     ↓
Peer Review
     ↓
Security Checks
     ↓
Approved Merge
```

This reduces unauthorized or accidental modifications.

---

# Build Environment Security

Even clean source code can produce malicious software if:

> The build environment is compromised.

Protect:

- Build servers
- Compilers
- Build scripts
- Dependencies
- Pipeline identities
- Signing keys

---

# Source Integrity vs. Build Integrity

```text
Source Integrity
→ Repository contains expected code.

Build Integrity
→ Build environment produces expected artifact.
```

Both are required.

---

# Compiler / Toolchain Risk

A malicious or compromised compiler could:

> Insert unwanted functionality into an otherwise clean application.

Therefore supply-chain security includes:

> Development and build tools themselves.

---

# Right to Audit

A right-to-audit clause gives an organization the contractual ability to:

> Verify supplier security practices.

Possible forms:

- Direct audit
- Third-party report
- Evidence review
- Assessment access

---

# Right-to-Audit Exam Clue

If a customer needs the ability to independently verify that a supplier follows required security controls:

> **Right to audit**

---

# 8.4 Ensure and Verify Supplier Security Requirements in Acquisition

Security requirements should be defined:

> Before purchasing or integrating the product.

Not after the contract is signed.

---

# Acquisition Security Workflow

```text
Define Requirements
      ↓
Evaluate Supplier
      ↓
Assess Product
      ↓
Negotiate Contract
      ↓
Acquire
      ↓
Verify Controls
      ↓
Monitor Supplier
```

---

# Supplier Security Requirements

Requirements may include:

- Secure software development practices
- Vulnerability management
- Incident reporting
- Patch timelines
- Security testing
- Logging
- Data handling
- Access control
- Encryption
- Compliance
- Audit rights

---

# Secure Development Practices

A supplier may be required to demonstrate:

- Secure coding standards
- Code review
- Security testing
- Vulnerability management
- Dependency management
- Secure release processes

---

# Security Policy Compliance Audit

An organization may assess whether supplier practices match:

> Contractual security requirements.

Examples:

```text
Required:
Secure code review

Supplier:
No review process
```

That represents:

> Supplier compliance risk.

---

# Vulnerability Notification

Contracts should define how suppliers communicate newly discovered vulnerabilities.

Questions include:

```text
Who gets notified?

How quickly?

What information is provided?

What workaround exists?

When will a fix be available?
```

---

# Incident Notification

Supplier incident obligations may define:

- Notification timeframe
- Contact method
- Required information
- Investigation cooperation
- Evidence preservation
- Regulatory coordination

---

# Exam Rule

"Notify us when convenient" is not a strong contractual requirement.

Use:

> Defined notification expectations.

---

# Incident Coordination

Some incidents affect:

- Supplier
- Customer
- Customer's customers
- Regulators

Contracts should define coordination responsibilities.

---

# Maintenance and Support Structure

Evaluate:

- Patch process
- Support period
- End-of-life policy
- Upgrade paths
- Response times

---

# EOL Risk

If a supplier discontinues support:

```text
No patches
   ↓
Known vulnerabilities remain
   ↓
Security risk increases
```

Contracts may therefore address:

- Minimum support periods
- EOL notification
- Migration support

---

# Licensing

Licensing can create operational and legal risk.

Examples:

- Restrictions on modification
- Redistribution rules
- Open-source obligations
- Usage limitations

---

# Security Track Record

Supplier evaluation should consider:

- Past vulnerabilities
- Response quality
- Incident transparency
- Patch timeliness
- Security maturity

### Exam Trap

Do not select a supplier solely because:

> It has never publicly disclosed a vulnerability.

That could mean:

- Excellent security

or:

- Poor disclosure practices.

Evaluate broader evidence.

---

# Scope of Testing

Determine:

> Who tests what?

This is particularly important with hosted/cloud services.

---

# Shared Responsibility Model

A supplier may secure:

- Infrastructure

while the customer secures:

- Identity
- Configuration
- Data
- Application settings

Exact responsibilities depend on service model and contract.

---

# Shared Responsibility Exam Trap

Wrong:

> We purchased SaaS, so the vendor is responsible for all security.

Better:

> Explicitly determine supplier and customer responsibilities.

---

# Security Testing Rights

Depending on the service, contracts may address:

- Penetration testing permission
- Testing windows
- Rules of engagement
- Notification requirements
- Testing boundaries

---

# Log Integration

Supplier-generated logs may need integration into the customer's:

> SIEM

Examples:

- Authentication events
- Administrative actions
- Security alerts
- Configuration changes

---

# SIEM Integration Requirement

If a cloud service is critical but produces no usable security telemetry:

> Detection and incident response may be weakened.

Therefore logging requirements should be considered during acquisition.

---

# Supplier Monitoring

Supplier evaluation should continue after purchase.

Monitor:

- Security advisories
- Ownership changes
- Certifications
- Incidents
- Vulnerabilities
- Support status
- Compliance status

---

# 8.5 Support Contractual Requirements

Contracts convert expectations into:

> Enforceable obligations.

Security professionals should understand important contractual concepts.

---

# Intellectual Property — IP

Contracts should define who owns:

- Source code
- Customizations
- Documentation
- Designs
- Data
- Derivative work

---

# IP Exam Clue

Question:

> Who owns code developed by an external contractor?

Think:

> **Intellectual property ownership should be explicitly addressed in the contract.**

Never assume ownership automatically.

---

# Customer Data Ownership

The contract should also define:

> Who owns customer data?

A supplier processing the data does not necessarily:

> Own the data.

---

# Code Escrow

Code escrow places software source code or related materials with:

> A trusted third party.

The escrow may be released under specified conditions.

---

# Why Code Escrow?

Suppose the vendor:

- Goes bankrupt
- Stops supporting the product
- Fails contractually
- Discontinues operations

The customer may require access to source code to:

> Maintain critical software.

---

# Code Escrow Mental Model

```text
Software Vendor
      ↓
Source Code
      ↓
Trusted Escrow Agent
      ↓
Released to Customer
only if contract conditions occur
```

---

# Code Escrow Exam Clue

If a company depends on proprietary software and worries that the vendor may:

> Go out of business

think:

> **Code escrow**

---

# Escrow Is Not Ownership

Important:

> Escrow does not automatically transfer ownership.

It provides conditional access according to:

> Contract terms.

---

# Liability

Liability determines:

> Who may be financially or legally responsible for loss or damage.

Example:

A software defect causes major financial loss.

The contract may define:

- Liability limits
- Indemnification
- Exclusions

---

# Risk Transfer

Contractual liability may transfer:

> Some financial risk.

But it does not magically eliminate:

> Operational security risk.

---

# Warranty

A warranty is a contractual promise concerning:

> Product condition, functionality, or defined characteristics.

Examples might include:

- Product substantially meets documentation
- Product contains no intentionally malicious functionality
- Vendor will correct specified defects

---

# Warranty vs. SLA

```text
Warranty
→ Promise about product/service condition or characteristics.

SLA
→ Measurable service performance commitment.
```

---

# EULA

**End-User License Agreement**

An EULA defines terms under which software may be:

- Used
- Installed
- Copied
- Modified
- Redistributed

Think:

> **Software usage/license terms**

---

# EULA vs. SLA

```text
EULA
→ How software may be used.

SLA
→ How service must perform.
```

---

# SLA

A **Service-Level Agreement** may define:

- Availability
- Response time
- Support
- Maintenance
- Security incident response
- Qualified personnel
- Remedies

---

# Example SLA

```text
Availability:
99.95%

Critical support response:
30 minutes

Critical security patch:
Within defined timeframe
```

---

# Security SLA

Security-related service levels may include:

- Vulnerability remediation timelines
- Incident notification
- Security response
- Certificate renewal
- Backup/recovery
- Logging availability

---

# Contract Exit Requirements

Contracts should consider what happens at termination.

Questions include:

```text
How is data returned?

How is vendor access removed?

How are credentials revoked?

How are customer copies deleted?

What happens to backups?

How is deletion verified?
```

---

# Data Return and Destruction

At termination, contracts may require the supplier to:

1. Return customer data
2. Destroy remaining copies
3. Provide evidence of destruction

---

# Vendor Lock-In

Contractual and technical dependencies may make switching vendors difficult.

Mitigate with:

- Data portability
- Standard formats
- Exit requirements
- Documentation
- Escrow where appropriate

---

# High-Value Exam Distinctions

## SBOM vs. SCA

```text
SBOM
→ What components exist?

SCA
→ What risks/issues exist in those components?
```

---

## Direct vs. Transitive Dependency

```text
Direct Dependency
→ Your application directly uses it.

Transitive Dependency
→ Your dependency depends on it.
```

---

## Pedigree vs. Provenance

```text
Pedigree
→ Historical background and quality.

Provenance
→ Origin of this specific artifact.
```

---

## Hash vs. Signature

```text
Hash
→ Detect modification.

Digital Signature
→ Detect modification + authenticate signer.
```

---

## Chain of Custody vs. Provenance

```text
Provenance
→ Where did it originate?

Chain of Custody
→ Who handled it over time?
```

---

## Repository Security vs. Build Security

```text
Repository Security
→ Protect source and version history.

Build Security
→ Protect transformation from source to artifact.
```

---

## Certification vs. Risk Assessment

```text
Certification
→ Evidence of evaluation against criteria.

Risk Assessment
→ Determine risk in your specific context.
```

Certification does not replace risk assessment.

---

## Community vs. Commercial Support

```text
Community
→ Community-driven maintenance.

Commercial
→ Contract-backed vendor maintenance.
```

Neither is automatically more secure.

---

## Supplier Assessment vs. Right to Audit

```text
Supplier Assessment
→ Evaluate supplier security.

Right to Audit
→ Contractual authority to verify it.
```

---

## Incident Notification vs. Vulnerability Notification

```text
Incident Notification
→ Supplier experienced a security event.

Vulnerability Notification
→ Supplier discovered a weakness affecting product/service.
```

---

## Warranty vs. Liability

```text
Warranty
→ What supplier promises about product/service.

Liability
→ Who bears consequences/costs when things go wrong.
```

---

## EULA vs. SLA

```text
EULA
→ Usage rights and restrictions.

SLA
→ Service performance obligations.
```

---

## SLA vs. Support Agreement

An SLA defines measurable service commitments.

A support agreement may define:

- Support availability
- Escalation
- Maintenance
- Assistance

They may overlap but are not identical.

---

# Common CSSLP Domain 8 Exam Traps

## Trap 1 — We Wrote the Code, So We Know Everything

Wrong:

> Our code is secure, so supply-chain risk is minimal.

Better:

> Examine dependencies, build tools, repositories, suppliers, and services.

---

## Trap 2 — SBOM Means Secure

Wrong:

> We have an SBOM, so our dependencies are secure.

Better:

> SBOM provides visibility; components still require risk analysis and monitoring.

---

## Trap 3 — Only Direct Dependencies Matter

Wrong:

> We track the five libraries developers selected.

Better:

> Transitive dependencies can also create exploitable risk.

---

## Trap 4 — Component Was Safe When Purchased

Wrong:

> We assessed it two years ago, so no further monitoring is necessary.

Better:

> New vulnerabilities and support changes require continuous monitoring.

---

## Trap 5 — Certification Eliminates Risk

Wrong:

> Vendor has a certification, therefore no further analysis is necessary.

Better:

> Evaluate certification scope and your specific risk.

---

## Trap 6 — Popular Repository Means Trusted Artifact

Wrong:

> Package came from the Internet's most popular repository, therefore authenticity is guaranteed.

Better:

> Verify provenance, integrity, and authenticity.

---

## Trap 7 — Matching Hash Proves Vendor Identity

Wrong:

> Matching hash proves the vendor produced the artifact.

Better:

> Hash supports integrity; trusted digital signatures provide stronger authenticity evidence.

---

## Trap 8 — Secure Source Means Secure Binary

Wrong:

> Repository was not modified, therefore the executable is trusted.

Better:

> Build environment compromise can alter output.

---

## Trap 9 — Supplier Security Is Procurement's Problem

Wrong:

> Security does not need involvement until after purchase.

Better:

> Define and evaluate security requirements during acquisition.

---

## Trap 10 — Vendor Responsible for Everything

Wrong:

> Hosted service means all security belongs to vendor.

Better:

> Define the shared responsibility model.

---

## Trap 11 — Contract Says "Secure"

Wrong:

> Vendor must maintain adequate security.

Better:

> Define measurable, auditable requirements where appropriate.

---

## Trap 12 — Vulnerability Notification Equals Incident Notification

Wrong:

> Vendor reports product vulnerabilities, therefore incident notification is covered.

Better:

> Contracts may need both.

---

## Trap 13 — Code Escrow Means Customer Owns Code

Wrong:

> Escrow means the customer receives full ownership immediately.

Better:

> Escrow provides conditional access based on contract terms.

---

## Trap 14 — Liability Prevents Incidents

Wrong:

> Vendor accepts liability, therefore technical risk is gone.

Better:

> Liability may transfer financial consequences; systems still require protection.

---

## Trap 15 — SLA Means Software Is Secure

Wrong:

> Vendor guarantees 99.99% availability, therefore security is excellent.

Better:

> Availability is one service metric; other security requirements still matter.

---

# Scenario Recognition Examples

## Scenario 1

A critical vulnerability is announced in a popular logging library.

The organization needs to determine which applications contain it.

Think:

> **SBOM / component inventory**

---

## Scenario 2

The organization knows it uses Library A but does not realize Library A depends on vulnerable Library B.

Think:

> **Transitive dependency risk**

---

## Scenario 3

Security wants automated detection of vulnerable open-source package versions.

Think:

> **Software Composition Analysis**

---

## Scenario 4

A component was approved last year but the maintainers have stopped releasing updates.

Think:

> **Component lifecycle / support risk**

---

## Scenario 5

A supplier presents an independent certification.

The customer still evaluates whether the certification scope applies to the purchased service.

Think:

> **Third-party assurance evidence + contextual risk assessment**

---

## Scenario 6

An administrator downloads a production package from an unofficial mirror.

Think:

> **Provenance risk**

---

## Scenario 7

A supplier provides a package plus SHA-256 checksum.

An attacker compromises the same download site and replaces both.

Best additional assurance:

> **Trusted digital signature**

---

## Scenario 8

A repository contains correct source code, but the build server has been compromised and inserts malicious code.

Think:

> **Build environment / supply-chain compromise**

---

## Scenario 9

A company needs to verify that its software supplier actually follows required secure development practices.

Think:

> **Right to audit / supplier assessment**

---

## Scenario 10

Vendor must inform customer within a defined period after discovering a security breach.

Think:

> **Incident notification requirement**

---

## Scenario 11

Vendor discovers a critical vulnerability in its software and must inform customers before a fix is released.

Think:

> **Vulnerability notification**

---

## Scenario 12

An application relies on an open-source project maintained by one volunteer who has stopped responding.

Think:

> **Maintenance/support risk**

---

## Scenario 13

The customer manages identities and application configuration while the SaaS provider manages infrastructure.

Think:

> **Shared responsibility model**

---

## Scenario 14

The organization requires vendor authentication and administrator activity logs to feed its central monitoring system.

Think:

> **SIEM/log integration requirement**

---

## Scenario 15

A company commissions custom software from an outside developer but never specifies who owns the resulting code.

Think:

> **Intellectual property ownership risk**

---

## Scenario 16

A company depends on proprietary software from a small supplier and is concerned the supplier could go bankrupt.

Think:

> **Code escrow**

---

## Scenario 17

A contract determines the maximum financial damages the supplier must pay after a security failure.

Think:

> **Liability**

---

## Scenario 18

A contract promises the software will substantially conform to defined specifications.

Think:

> **Warranty**

---

## Scenario 19

A document defines the conditions under which customers may install and use purchased software.

Think:

> **EULA**

---

## Scenario 20

A provider promises 99.99% uptime and a 30-minute response to critical incidents.

Think:

> **SLA**

---

# Software Supply Chain Workflow

```text
Identify Need
    ↓
Define Security Requirements
    ↓
Identify Candidate Component/Supplier
    ↓
Assess Security
    ↓
Verify Pedigree & Provenance
    ↓
Review Contract
    ↓
Acquire
    ↓
Integrate
    ↓
Maintain SBOM
    ↓
Monitor Vulnerabilities
    ↓
Update / Replace
```

---

# Component Selection Checklist

```text
[ ] Is the component actually necessary?

[ ] Is it actively maintained?

[ ] Is the current version supported?

[ ] Are known vulnerabilities acceptable?

[ ] Who maintains it?

[ ] What is its security history?

[ ] What privileges does it require?

[ ] What data can it access?

[ ] What dependencies does it introduce?

[ ] Is licensing acceptable?

[ ] Can we verify its origin?

[ ] Can we verify artifact integrity?

[ ] Can we update or replace it?

[ ] Is it included in the SBOM?
```

---

# Supplier Assessment Checklist

```text
[ ] Secure development practices reviewed?

[ ] Security certifications reviewed?

[ ] Certification scope validated?

[ ] Assessment reports reviewed?

[ ] Security track record evaluated?

[ ] Vulnerability process evaluated?

[ ] Incident-response process evaluated?

[ ] Patch/support lifecycle understood?

[ ] Shared responsibility documented?

[ ] Testing responsibilities documented?

[ ] Audit rights established?

[ ] Log/SIEM integration addressed?

[ ] End-of-life policy understood?
```

---

# Provenance Checklist

```text
[ ] Official source verified?

[ ] Artifact version verified?

[ ] Cryptographic hash checked?

[ ] Digital signature checked?

[ ] Signer identity trusted?

[ ] Repository protected?

[ ] Build environment protected?

[ ] Chain of custody documented where needed?
```

---

# Contract Checklist

```text
[ ] Security requirements defined?

[ ] Vulnerability notification timeframe?

[ ] Incident notification timeframe?

[ ] Patch/remediation expectations?

[ ] Audit rights?

[ ] Testing rights?

[ ] Data ownership?

[ ] Intellectual property ownership?

[ ] Data return requirements?

[ ] Data destruction requirements?

[ ] Logging requirements?

[ ] Support lifecycle?

[ ] End-of-life notification?

[ ] Code escrow where appropriate?

[ ] Liability?

[ ] Warranty?

[ ] EULA terms?

[ ] SLA requirements?
```

---

# Fast Memory Sheet

```text
SUPPLY CHAIN RISK MANAGEMENT
→ Manage risk from components, suppliers, tools, and dependencies.

COMPONENT SELECTION
→ Evaluate before integrating.

SBOM
→ What software is inside our software?

SCA
→ Analyze components for vulnerabilities/licenses.

TRANSITIVE DEPENDENCY
→ Dependency of a dependency.

CONTINUOUS MONITORING
→ Approved today does not mean secure tomorrow.

CERTIFICATION
→ Evidence, not a guarantee.

ASSESSMENT REPORT
→ Evidence about implemented controls.

PROVENANCE
→ Where did this artifact come from?

PEDIGREE
→ What is its history/background?

CHAIN OF CUSTODY
→ Who handled the artifact?

HASH
→ Integrity.

DIGITAL SIGNATURE
→ Integrity + authenticity.

REPOSITORY SECURITY
→ Protect source and versions.

BUILD ENVIRONMENT SECURITY
→ Protect creation of final artifacts.

RIGHT TO AUDIT
→ Contractual ability to verify supplier controls.

VULNERABILITY NOTIFICATION
→ Supplier tells us about product weakness.

INCIDENT NOTIFICATION
→ Supplier tells us about security incident.

SHARED RESPONSIBILITY
→ Define supplier/customer security duties.

SIEM INTEGRATION
→ Supplier logs support centralized detection.

IP OWNERSHIP
→ Who owns code/data/intellectual property?

CODE ESCROW
→ Trusted third party holds source for defined release conditions.

LIABILITY
→ Who bears contractual consequences?

WARRANTY
→ Supplier promise about product/service.

EULA
→ Software usage terms.

SLA
→ Measurable service commitments.
```

---

# Hardest Domain 8 Distinctions to Memorize

| Question Is Asking... | Likely Concept |
|---|---|
| What components are included? | **SBOM** |
| Which components have vulnerabilities? | **SCA** |
| Vulnerability in dependency-of-dependency? | **Transitive Dependency** |
| Where did artifact originate? | **Provenance** |
| What is component's historical background? | **Pedigree** |
| Who handled artifact? | **Chain of Custody** |
| Was artifact modified? | **Hash** |
| Who signed artifact + was it modified? | **Digital Signature** |
| Can customer verify supplier controls? | **Right to Audit** |
| Supplier discovers product weakness? | **Vulnerability Notification** |
| Supplier suffers compromise? | **Incident Notification** |
| Who secures which part of cloud service? | **Shared Responsibility** |
| Supplier logs must reach SOC? | **SIEM Integration** |
| Who owns custom-developed code? | **IP Ownership** |
| Supplier goes bankrupt and source is needed? | **Code Escrow** |
| Who pays after failure? | **Liability** |
| Supplier promise about product quality? | **Warranty** |
| Rules governing software use? | **EULA** |
| Uptime and response commitments? | **SLA** |

---

# CSSLP Domain 8 Exam Strategy

Domain 8 questions often ask you to think like:

> **A security professional involved in procurement, architecture, development, operations, and vendor management at the same time.**

The key principle is:

> You cannot outsource accountability merely by outsourcing software.

---

# FIRST Exam Logic

If the organization plans to acquire a new third-party component, do not wait until:

> After the contract is signed

to identify security requirements.

Better sequence:

```text
Define Requirements
      ↓
Assess Supplier
      ↓
Assess Component
      ↓
Negotiate Security Terms
      ↓
Acquire
```

---

# Example

Question:

> An organization plans to acquire a third-party payment platform. What should the security team do FIRST?

Weak:

> Connect the platform and scan it afterward.

Better:

> Define applicable security, privacy, operational, and supplier requirements before acquisition.

---

# BEST Exam Logic

If two answers seem correct, prefer the one that provides:

1. Visibility
2. Verifiable evidence
3. Continuous monitoring
4. Explicit responsibility
5. Contractual enforceability
6. Traceability

---

# Example

A supplier claims:

> Our product follows secure coding practices.

Possible responses:

A. Accept the statement because supplier is reputable  
B. Require objective evidence or contractual audit rights  
C. Deploy product and wait for incidents  
D. Require users to change passwords frequently  

Best:

> **B**

Why?

Because supply-chain assurance should rely on:

> Verifiable evidence rather than unsupported trust.

---

# Trust but Verify

A useful Domain 8 principle:

```text
Supplier Says It
      ↓
Obtain Evidence
      ↓
Verify Scope
      ↓
Contract Requirement
      ↓
Monitor Continuously
```

---

# Visibility Before Vulnerability

You cannot quickly respond to a vulnerable component if:

> You do not know you are using it.

Therefore:

```text
Inventory
   ↓
Monitor
   ↓
Prioritize
   ↓
Remediate
```

This is why SBOM and component inventory are important.

---

# Supply Chain Blast Radius

A popular component may be used in:

```text
Application A
Application B
Application C
Application D
```

One upstream compromise can therefore affect:

> Many downstream systems simultaneously.

This is a key supply-chain risk characteristic.

---

# Dependency Concentration Risk

Using one third-party component across many products can provide:

- Consistency
- Efficiency

but also create:

> A large shared blast radius.

Security decisions should balance:

- Reuse
- Trust
- Isolation
- Monitoring

---

# Artifact Trust Sequence

When receiving software:

```text
Where did it come from?
        ↓
Is the source trusted?
        ↓
Was the artifact altered?
        ↓
Who signed it?
        ↓
Do we trust the signer?
        ↓
Is this the expected version?
```

---

# Supplier Failure Planning

Ask:

> What happens if the supplier disappears tomorrow?

Consider:

- Source access
- Support
- Updates
- Data export
- Credentials
- Migration
- Documentation

For critical proprietary software:

> Code escrow may be appropriate.

---

# Security Responsibilities Must Be Explicit

Do not assume:

```text
Vendor handles security.
```

Instead define:

```text
Vendor responsibility
        +
Customer responsibility
        =
Complete security model
```

Especially for:

- Cloud
- SaaS
- Managed services
- Outsourced development

---

# Domain 8 Master Sequence

Memorize:

```text
Identify
   ↓
Select
   ↓
Assess
   ↓
Inventory
   ↓
Verify
   ↓
Contract
   ↓
Integrate
   ↓
Monitor
   ↓
Update
   ↓
Audit
```

---

# Final Exam Rules

### Rule 1

> Third-party components create security risk even when your own source code is secure.

### Rule 2

> Assess components before adoption and continue monitoring them afterward.

### Rule 3

> SBOM provides component visibility; it does not guarantee component security.

### Rule 4

> Track relevant transitive dependencies, not only direct dependencies.

### Rule 5

> Certification and assessment reports provide assurance evidence but do not eliminate contextual risk analysis.

### Rule 6

> Provenance identifies where an artifact came from; pedigree describes its historical background.

### Rule 7

> Hashes support integrity; digital signatures add stronger authenticity assurance.

### Rule 8

> Protect both repositories and build environments.

### Rule 9

> Secure source code does not guarantee a trustworthy binary if the build environment is compromised.

### Rule 10

> Right-to-audit provisions allow organizations to verify supplier security obligations.

### Rule 11

> Define supplier security requirements before acquisition, not after deployment.

### Rule 12

> Vulnerability notification and incident notification are different obligations.

### Rule 13

> Evaluate supplier maintenance and support lifecycle, including end-of-life risk.

### Rule 14

> Explicitly define the shared responsibility model.

### Rule 15

> Supplier security telemetry may need integration into organizational monitoring and SIEM.

### Rule 16

> Intellectual-property ownership should be defined contractually.

### Rule 17

> Code escrow provides conditional access to source code if specified events occur; it does not automatically transfer ownership.

### Rule 18

> Liability addresses responsibility for consequences; warranty addresses promises about the product or service.

### Rule 19

> EULA defines software usage terms; SLA defines measurable service commitments.

### Rule 20

> Outsourcing a service does not automatically outsource security accountability.

### Rule 21

> If two answers appear correct, prefer the option that provides verifiable evidence, explicit responsibility, continuous monitoring, and enforceable security requirements.

---

# Final CSSLP 8-Domain Map

```text
Domain 1
Secure Software Concepts
        ↓
Understand security principles

Domain 2
Secure Software Lifecycle Management
        ↓
Manage security throughout SDLC

Domain 3
Secure Software Requirements
        ↓
Define what security is required

Domain 4
Secure Software Architecture and Design
        ↓
Design how security will work

Domain 5
Secure Software Implementation
        ↓
Build software securely

Domain 6
Secure Software Testing
        ↓
Verify security works

Domain 7
Secure Software Deployment, Operations, Maintenance
        ↓
Operate and maintain securely

Domain 8
Secure Software Supply Chain
        ↓
Secure everything you depend on
```

---

# One-Line Memory Aid for All Domains

```text
D1 → Know security.

D2 → Manage security.

D3 → Require security.

D4 → Design security.

D5 → Build security.

D6 → Test security.

D7 → Operate security.

D8 → Trust the supply chain carefully.
```

---

## Disclaimer

This is an independent CSSLP study guide and is not an official ISC2 publication or a collection of official exam questions.
