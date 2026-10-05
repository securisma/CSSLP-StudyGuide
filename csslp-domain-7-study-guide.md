# CSSLP Domain 7 Study Guide
## Secure Software Deployment, Operations, Maintenance

This study guide focuses on **CSSLP Domain 7: Secure Software Deployment, Operations, Maintenance**.

Domain 7 covers what happens **after software has been built and tested**: secure deployment, configuration, operation, monitoring, incident handling, patching, vulnerability management, runtime protection, resilience, and service-level commitments.

The major areas are:

1. Operational risk analysis
2. Secure configuration and version control
3. Secure software release
4. Security data and secrets management
5. Secure installation
6. Security approval to operate
7. Continuous security monitoring
8. Incident response
9. Patch management
10. Vulnerability management
11. Runtime protection
12. Continuity of operations
13. Service-level objectives and agreements

---

# Domain 7 Mental Model

Think of Domain 7 as:

```text
Build
  ↓
Release
  ↓
Deploy Securely
  ↓
Harden
  ↓
Approve
  ↓
Monitor
  ↓
Detect
  ↓
Respond
  ↓
Patch
  ↓
Manage Vulnerabilities
  ↓
Recover / Maintain Continuity
```

Domain 6 asks:

> Is the software ready from a testing perspective?

Domain 7 asks:

> How do we securely deploy, operate, monitor, maintain, and recover it?

---

# Fast Exam Recognition Table

| If You See... | Think... |
|---|---|
| Risks introduced by production environment | **Operational Risk Analysis** |
| Production differs from QA/staging | **Environment Risk** |
| Administrators require specialized training | **Personnel Training** |
| Production operation must meet privacy law | **Legal Compliance** |
| Multiple systems interact in production | **System Integration Risk** |
| Known secure configuration | **Security Baseline** |
| Track configuration changes | **Configuration Management** |
| Track source/configuration versions | **Version Control** |
| Release through automated security pipeline | **Secure CI/CD / DevSecOps** |
| Verify release artifact has not changed | **Hash / Code Signing** |
| Password/API key stored securely | **Secrets Management** |
| Private cryptographic material | **Key Management** |
| Certificate lifecycle | **Certificate Management** |
| Remove unnecessary services | **Hardening** |
| Minimum permissions during deployment | **Least Privilege** |
| Automated infrastructure provisioning | **Infrastructure as Code (IaC)** |
| Formal decision allowing production use | **Approval to Operate / Risk Acceptance** |
| Logs, metrics, traces, events | **Observability** |
| Aggregate security events | **SIEM** |
| Detect hostile behavior | **IDS / Monitoring** |
| New intelligence about active attackers | **Threat Intelligence** |
| Determine seriousness of incident | **Triage** |
| Preserve and analyze evidence | **Forensics** |
| Fix immediate incident damage | **Remediation** |
| Determine why incident occurred | **Root Cause Analysis** |
| Test and deploy security updates | **Patch Management** |
| Track CVEs and remediation status | **Vulnerability Management** |
| App protects itself while running | **RASP** |
| Filter malicious web requests | **WAF** |
| Randomize memory addresses | **ASLR** |
| Prevent execution in data memory | **DEP / Dynamic Execution Prevention** |
| Restore data | **Backup** |
| Recover technology after disaster | **DRP** |
| Continue critical business operations | **BCP** |
| Continue service despite component failure | **Resiliency / Redundancy** |
| Required uptime or response commitment | **SLA / SLO** |

---

# 7.1 Perform Operational Risk Analysis

Operational risk analysis evaluates security risks introduced when software is deployed into its actual operating environment.

The application may have passed development and testing but still encounter new risks because of:

- Production infrastructure
- Personnel
- Integrations
- Configuration
- Legal requirements
- External dependencies
- Operational processes

---

# Development Risk vs. Operational Risk

Development risk:

> Security weakness exists in the software itself.

Operational risk:

> Risk emerges because of how the software is deployed or operated.

Example:

```text
Application:
Secure authentication implemented correctly.

Production:
Administrator disables MFA for convenience.
```

The software may be secure by design, but the operational configuration introduces risk.

---

# Deployment Environments

Common environments include:

```text
Development
     ↓
QA / Testing
     ↓
Staging
     ↓
Production
```

Each environment has different:

- Access
- Data
- Security controls
- Availability requirements
- Users

---

# Production Environment

Production usually requires the strongest operational controls because it contains:

- Real users
- Real business processes
- Real data
- Real financial/reputational consequences

### Exam Rule

Do not assume:

> A setting acceptable in development is acceptable in production.

---

# Example

Development:

```text
Debug mode = ON
Verbose errors = ON
Test account = ENABLED
```

Production should generally use:

```text
Debug mode = OFF
Verbose sensitive errors = OFF
Test accounts = REMOVED
```

---

# Environment Separation

Development, QA, staging, and production environments should generally be appropriately separated.

Why?

To reduce:

- Unauthorized production access
- Accidental deployment
- Production-data leakage
- Cross-environment credential reuse

---

# Production Data in Non-Production

Copying production data to QA creates security and privacy risk.

Better options may include:

- Synthetic data
- Anonymized data
- Tokenized data
- Sanitized datasets

Remember from Domain 6:

> Sensitive data remains sensitive even in a test environment.

---

# Personnel Training

Administrators and users have different training requirements.

## Administrators

May need training in:

- Secure configuration
- Patch deployment
- Incident handling
- Secrets management
- Logging
- Backup/recovery
- Privileged access

## Users

May need training in:

- Secure application use
- Authentication
- Privacy
- Reporting suspicious activity

---

# Exam Principle

Security technology alone is insufficient if:

> Operators do not know how to securely administer it.

---

# Legal Compliance

Operational security must continue to satisfy:

- Regulations
- Privacy requirements
- Copyright
- Contracts
- Organizational policy
- Retention requirements

### Exam Trap

Compliance does not end after development.

Operations must continue complying throughout the software lifecycle.

---

# System Integration Risk

Security behavior may change when systems are connected.

Example:

```text
Application A
      ↓
Identity Provider
      ↓
Application B
      ↓
Payment Processor
```

Operational risks include:

- Authentication assumptions
- Key sharing
- Availability dependencies
- Data exposure
- Protocol compatibility
- Failure propagation

---

# 7.2 Secure Configuration and Version Control

Configuration management ensures systems remain in an approved and understood state.

Security-relevant configuration includes:

- Operating systems
- Applications
- Databases
- Network devices
- Cloud resources
- Containers
- Hardware
- Security controls

---

# Security Baseline

A baseline is:

> An approved known-good configuration.

Example:

```text
Production Web Server Baseline

SSH root login       = Disabled
TLS old protocols    = Disabled
Firewall             = Enabled
Debug mode           = Disabled
Default accounts     = Removed
Logging              = Enabled
```

---

# Baseline Purpose

A baseline allows you to answer:

> What should this system look like?

Current configuration can then be compared against the baseline.

---

# Configuration Drift

Configuration drift occurs when systems gradually diverge from their approved baseline.

Example:

```text
Day 1:
Secure baseline

Day 30:
Administrator opens temporary port

Day 60:
Debug mode enabled

Day 90:
Temporary changes never removed
```

Think:

> **Configuration drift**

---

# Exam Clue

If servers that were initially identical gradually develop inconsistent settings:

> Configuration management / drift problem.

---

# Hardware Configuration

Hardware may also require secure configuration.

Examples:

- Firmware
- BIOS/UEFI
- Secure boot
- TPM
- Hardware security modules
- Management interfaces

---

# Version Control

Version control helps track:

- Source code
- Configuration
- Infrastructure definitions
- Documentation
- Deployment scripts

Benefits include:

- Auditability
- Rollback
- Change history
- Accountability

---

# Version Control Security

Protect repositories with:

- Authentication
- Authorization
- Branch protection
- Audit logging
- Signed commits where appropriate
- Secrets scanning

---

# Exam Trap

Version control should not become a secrets vault.

Do not commit:

```text
Passwords
API keys
Private keys
Production tokens
```

---

# Patching and Version Control

Organizations should know:

- What version is deployed
- Which patches are installed
- Which dependencies are present

Without inventory and version knowledge:

> Effective vulnerability and patch management becomes difficult.

---

# Documentation Practices

Configuration documentation should describe:

- Current approved state
- Dependencies
- Changes
- Deployment process
- Recovery procedures

### Exam Principle

Documentation should match:

> The actual deployed environment.

---

# 7.3 Release Software Securely

Software release is the transition from:

> Tested artifact

to:

> Approved deployable artifact.

Release security should protect:

- Integrity
- Authenticity
- Traceability

---

# Secure CI/CD Pipeline

A CI/CD pipeline may perform:

```text
Commit
  ↓
Build
  ↓
SAST
  ↓
SCA
  ↓
Tests
  ↓
Artifact
  ↓
Signature
  ↓
Deployment
```

The pipeline itself is highly privileged.

---

# CI/CD Security

Protect:

- Repository access
- Build servers
- Pipeline credentials
- Artifact repositories
- Deployment credentials
- Signing keys

---

# Exam Principle

A compromised CI/CD system can produce malicious software even when:

> The source repository itself is clean.

---

# DevSecOps

DevSecOps integrates security into development and operational workflows.

Think:

```text
Development
+
Security
+
Operations
```

Security becomes:

> Continuous and automated where practical.

---

# Application Security Toolchain

A security toolchain may include:

- SAST
- DAST
- SCA
- Secrets scanning
- IaC scanning
- Container scanning
- Artifact verification

### Exam Trap

One tool does not provide complete assurance.

Use complementary techniques.

---

# Build Artifact Verification

Before deployment, verify:

> Is this the same artifact that was approved?

---

# Cryptographic Hash

A hash can detect modification.

```text
Artifact
   ↓
SHA-256
   ↓
Digest
```

If artifact changes:

> The digest changes.

---

# Code Signing

Code signing helps provide:

- Integrity
- Authenticity

Conceptually:

```text
Build Artifact
     ↓
Hash
     ↓
Sign using protected private key
     ↓
Signed Artifact
```

---

# Hash vs. Digital Signature

```text
Hash
→ Was the artifact changed?

Digital Signature
→ Was it changed AND who signed it?
```

A hash alone is insufficient if an attacker can replace:

> Both the artifact and hash.

---

# 7.4 Store and Manage Security Data

Security-sensitive data includes:

- Credentials
- Secrets
- Cryptographic keys
- Certificates
- Configuration

These must be protected throughout their lifecycle.

---

# Credentials

Credentials may include:

- Passwords
- Tokens
- API keys
- Service credentials

Avoid:

- Hard-coding
- Plaintext storage
- Sharing
- Long-lived unmanaged credentials

---

# Secrets Management

A secrets-management system can provide:

- Controlled storage
- Access control
- Rotation
- Auditing
- Expiration
- Dynamic credentials

---

# Secret Lifecycle

```text
Generate
   ↓
Store
   ↓
Distribute
   ↓
Use
   ↓
Rotate
   ↓
Revoke
   ↓
Destroy
```

---

# Key Management

Cryptographic key security depends heavily on proper lifecycle management.

Consider:

- Generation
- Storage
- Distribution
- Rotation
- Revocation
- Backup
- Destruction

---

# Key Storage

Private keys should be protected using appropriate mechanisms such as:

- HSM
- TPM
- Secure element
- Protected key vault

---

# Certificate Management

Certificates have lifecycles.

```text
Issue
 ↓
Deploy
 ↓
Monitor Expiration
 ↓
Renew / Rotate
 ↓
Revoke
```

Expired certificates can cause:

> Availability failures.

Compromised certificates can create:

> Authentication and trust failures.

---

# Configuration as Sensitive Data

Configuration may itself contain:

- Database addresses
- Credentials
- Internal hostnames
- Feature flags
- Security settings

Treat sensitive configuration according to risk.

---

# 7.5 Ensure Secure Installation

Secure installation ensures the approved software is deployed in a secure state.

---

# Secure Installation Flow

```text
Verify Artifact
      ↓
Provision Environment
      ↓
Apply Secure Baseline
      ↓
Install
      ↓
Configure Least Privilege
      ↓
Harden
      ↓
Validate
```

---

# Secure Boot

Secure boot helps verify that trusted software is loaded during startup.

Conceptually:

```text
Root of Trust
     ↓ verifies
Bootloader
     ↓ verifies
OS / Firmware
```

---

# Least Privilege

Applications should run with:

> Only the permissions they require.

Bad:

```text
Web Application
→ Runs as root / administrator
```

Better:

```text
Web Application
→ Dedicated restricted service identity
```

---

# Environment Hardening

Hardening reduces attack surface.

Examples:

- Remove unused services
- Disable default accounts
- Close unused ports
- Apply security patches
- Restrict file permissions
- Disable debug features
- Configure firewall

---

# Hardening Principle

Think:

> **Remove what is unnecessary and restrict what remains.**

---

# Secure Patching and Updates

Updates should be:

- Authenticated
- Integrity verified
- Tested
- Approved
- Deployable with rollback where appropriate

---

# Secure Provisioning

Provisioning may include:

- Credentials
- Configuration
- Licensing
- Infrastructure

Automation can improve consistency.

---

# Infrastructure as Code — IaC

IaC defines infrastructure through code.

Benefits:

- Repeatability
- Version control
- Auditability
- Consistency

Risks:

- Insecure templates
- Excessive permissions
- Hard-coded secrets

---

# IaC Exam Principle

IaC can reduce manual configuration drift, but:

> An insecure template can reproduce the same vulnerability everywhere.

---

# Security Policy Implementation

Operational security policy should be translated into technical configuration.

Example:

Policy:

> Administrative interfaces must not be publicly accessible.

Implementation:

```text
Management network
+
Firewall restriction
+
MFA
```

---

# 7.6 Obtain Security Approval to Operate

Before production use, appropriate authority may need to formally approve the system.

Think:

> **Is the remaining risk acceptable?**

---

# Approval Process

Conceptually:

```text
Security Assessment
      ↓
Known Findings
      ↓
Residual Risk
      ↓
Risk Decision
      ↓
Approve / Reject Operation
```

---

# Risk Acceptance

Risk acceptance is:

> A management decision.

It should be made by someone with:

- Appropriate authority
- Risk ownership
- Understanding of impact

---

# Critical Exam Rule

A developer or security tester generally should not independently decide:

> This business risk is acceptable.

They provide information.

The appropriate authority:

> Accepts the residual risk.

---

# Sign-Off

Formal approval may document:

- Known risks
- Exceptions
- Compensating controls
- Conditions
- Expiration/review dates

---

# 7.7 Perform Information Security Continuous Monitoring

Security monitoring continues after deployment.

Goal:

> Detect changes, attacks, failures, and emerging risk.

---

# Observable Data

Useful operational information includes:

- Logs
- Events
- Metrics
- Traces
- Telemetry

---

# Logs

Logs record discrete events.

Example:

```text
User login failed
Admin role changed
Firewall rule modified
```

---

# Metrics

Metrics are numerical measurements.

Examples:

```text
CPU utilization = 80%
Failed logins = 900/hour
API error rate = 12%
```

---

# Traces

Traces follow transactions through distributed systems.

Example:

```text
API Gateway
    ↓
Order Service
    ↓
Payment Service
    ↓
Database
```

Useful for:

- Troubleshooting
- Performance
- Security investigations

---

# Telemetry

Telemetry is operational data automatically collected from systems.

It may include:

- Health
- Performance
- Security activity
- Network behavior

---

# Observability

Observability asks:

> Can we understand what is happening inside the system using its outputs?

Common pillars:

```text
Logs
Metrics
Traces
```

---

# Threat Intelligence

Threat intelligence provides information about:

- Attackers
- Campaigns
- Vulnerabilities
- Indicators of compromise
- Tactics and techniques

Operational teams can use it to:

> Improve detection and prioritization.

---

# Intrusion Detection

IDS attempts to identify malicious or suspicious behavior.

Possible forms:

- Network-based
- Host-based

---

# Detection vs. Prevention

```text
IDS
→ Detect / alert

IPS
→ Detect + attempt to block
```

---

# SIEM

**Security Information and Event Management**

SIEM platforms aggregate and correlate security data.

Example:

```text
Firewall Logs
      +
Application Logs
      +
Identity Logs
      +
Endpoint Events
      ↓
     SIEM
      ↓
Correlation / Alerting
```

---

# SIEM Exam Clue

If the organization wants to:

> Centrally correlate security events across many systems

think:

> **SIEM**

---

# Regulation and Privacy Changes

Continuous monitoring is not only technical.

Organizations should monitor changes in:

- Laws
- Regulations
- Privacy requirements
- Contracts

because those may affect operational security requirements.

---

# 7.8 Execute the Incident Response Plan

An incident response plan defines what to do when a security incident occurs.

Key Domain 7 topics include:

- Triage
- Forensics
- Remediation
- Root cause analysis

---

# Incident Triage

Triage determines:

- Is this actually an incident?
- How severe is it?
- What is affected?
- What requires immediate attention?

Think:

> **Prioritize and classify the incident.**

---

# Incident Response Flow

A simplified model:

```text
Detect
  ↓
Triage
  ↓
Contain
  ↓
Investigate
  ↓
Remediate
  ↓
Recover
  ↓
Root Cause Analysis
  ↓
Lessons Learned
```

---

# Containment

Containment limits additional damage.

Examples:

- Isolate host
- Disable account
- Block malicious IP
- Revoke token

---

# Forensics

Digital forensics involves:

- Identifying evidence
- Preserving evidence
- Collecting evidence
- Analyzing evidence

---

# Evidence Integrity

Evidence should be protected against alteration.

Possible practices:

- Cryptographic hashes
- Chain of custody
- Controlled access
- Forensic copies

---

# Chain of Custody

Chain of custody records:

> Who possessed or handled evidence and when.

Important when evidence may be used for:

- Legal proceedings
- Disciplinary action
- Regulatory investigations

---

# Remediation

Remediation fixes the immediate security problem.

Examples:

- Remove malware
- Reset credentials
- Patch vulnerability
- Correct configuration

---

# Root Cause Analysis

Root cause analysis asks:

> Why did the incident happen?

Example:

Immediate cause:

> Attacker exploited SQL injection.

Root cause:

> Development standard did not require parameterized queries and security testing did not cover the endpoint.

---

# Root Cause vs. Remediation

```text
Remediation
→ Fix the current problem.

Root Cause Analysis
→ Determine why the problem existed.
```

Both matter.

---

# Lessons Learned

After an incident:

- Improve controls
- Update standards
- Improve training
- Add tests
- Improve monitoring

Goal:

> Prevent recurrence.

---

# 7.9 Perform Patch Management

Patch management is the controlled process of evaluating, testing, deploying, and verifying updates.

---

# Patch Management Flow

```text
Patch Released
     ↓
Identify Affected Systems
     ↓
Assess Risk
     ↓
Test Patch
     ↓
Approve
     ↓
Deploy
     ↓
Verify
     ↓
Monitor
```

---

# Why Test Patches?

A patch may:

- Break compatibility
- Cause outages
- Change configuration
- Introduce new issues

Therefore:

> Test according to system criticality and urgency.

---

# Emergency Patching

A critical actively exploited vulnerability may require accelerated deployment.

However:

> Emergency does not mean uncontrolled.

Use:

> Approved emergency change procedures.

---

# Patch Prioritization

Consider:

- Severity
- Exploitability
- Exposure
- Active exploitation
- Asset criticality
- Business impact

---

# Exam Trap

Do not prioritize patches based only on:

> Publication date or CVSS score.

Risk context matters.

---

# Rollback

Patch planning should consider:

> What happens if the update fails?

A rollback plan can support operational resilience.

---

# 7.10 Perform Vulnerability Management

Vulnerability management is broader than patch management.

It includes:

```text
Discover
  ↓
Validate
  ↓
Classify
  ↓
Prioritize
  ↓
Remediate
  ↓
Verify
  ↓
Track
```

---

# Patch Management vs. Vulnerability Management

```text
Patch Management
→ Manage software updates.

Vulnerability Management
→ Manage security weaknesses.
```

Not every vulnerability has:

> A vendor patch.

Possible responses may include:

- Configuration change
- Compensating control
- Feature removal
- Upgrade
- Risk acceptance

---

# CVE

**Common Vulnerabilities and Exposures**

A CVE identifier refers to:

> A specific publicly known vulnerability.

Example format:

```text
CVE-YYYY-NNNNN
```

---

# CVE vs. CVSS

```text
CVE
→ Identifies the vulnerability.

CVSS
→ Scores technical severity.
```

---

# Vulnerability Triage

Triage considers:

- Is the finding valid?
- Is our system affected?
- Is it exploitable?
- Is it exposed?
- What is the impact?

---

# Vulnerability Inventory

To manage vulnerabilities effectively, you need to know:

> What assets and versions you actually have.

This connects to:

- Asset inventory
- Configuration management
- SBOM
- Version control

---

# Remediation Verification

After remediation:

> Verify the vulnerability is actually resolved.

Do not close it merely because:

> A patch was installed.

---

# 7.11 Incorporate Runtime Protection

Runtime protection provides defenses while software is executing.

Examples include:

- RASP
- WAF
- ASLR
- DEP

---

# RASP

**Runtime Application Self-Protection**

RASP operates within or closely integrated with the application runtime.

It can monitor application behavior and potentially block attacks.

Think:

> **Application-aware runtime protection**

---

# RASP Example

```text
Request
  ↓
Application
  ↓
RASP detects dangerous SQL behavior
  ↓
Block / Alert
```

---

# WAF

**Web Application Firewall**

A WAF monitors and filters web traffic.

```text
Internet
   ↓
WAF
   ↓
Web Application
```

It may detect/block:

- Injection attempts
- Malicious request patterns
- Known attacks

---

# WAF Limitation

A WAF should not be considered a permanent substitute for:

> Fixing vulnerable application code.

Think:

> **Defense in depth / compensating control**

---

# RASP vs. WAF

```text
WAF
→ Protects at the web traffic boundary.

RASP
→ Protects from within/near application runtime.
```

---

# ASLR

**Address Space Layout Randomization**

ASLR randomizes memory locations.

Goal:

> Make memory-exploitation attacks less predictable.

---

# ASLR Example

Without ASLR:

```text
Library always at same memory address
```

With ASLR:

```text
Memory locations vary between executions
```

---

# DEP

**Data Execution Prevention**

DEP prevents certain memory regions intended for data from being executed as code.

Think:

> **Data memory should not become executable code.**

---

# ASLR vs. DEP

```text
ASLR
→ Randomize memory locations.

DEP
→ Prevent execution from data regions.
```

They can work together as:

> Defense in depth.

---

# Runtime Controls Are Not Root-Cause Fixes

If an application contains SQL injection:

WAF/RASP may reduce exploitation.

But the best long-term remediation is still:

> Fix the vulnerable code.

---

# 7.12 Support Continuity of Operations

Continuity addresses the organization's ability to withstand and recover from disruption.

Topics include:

- Backup
- Archiving
- Retention
- Disaster recovery
- Resiliency
- Business continuity

---

# Backup

A backup provides a copy of data for recovery.

Security considerations:

- Encryption
- Access control
- Integrity
- Offsite protection
- Restore testing

---

# Critical Rule

A backup is not useful unless:

> It can successfully be restored.

Therefore:

> Test restoration.

---

# Backup vs. Redundancy

```text
Backup
→ Recover lost/corrupted data.

Redundancy
→ Keep service operating during failure.
```

---

# Archiving

Archiving preserves information for:

- Long-term retention
- Compliance
- Historical purposes

Archive ≠ backup.

---

# Backup vs. Archive

```text
Backup
→ Operational recovery.

Archive
→ Long-term preservation.
```

---

# Retention

Retention determines:

> How long information should be kept.

It should follow:

- Legal requirements
- Regulatory requirements
- Business requirements
- Privacy requirements

---

# Disaster Recovery Plan — DRP

DRP focuses primarily on:

> Restoring technology and systems after disruption.

Examples:

- Restore data center
- Recover servers
- Restore applications
- Recover databases

---

# Business Continuity Plan — BCP

BCP focuses on:

> Keeping critical business functions operating.

It is broader than IT recovery.

---

# DRP vs. BCP

```text
DRP
→ Restore technology.

BCP
→ Continue the business.
```

---

# Example

Data center fails.

DRP asks:

> How do we restore the payment systems?

BCP asks:

> How does the organization continue accepting and processing critical payments while recovery occurs?

---

# Resiliency

Resiliency is the ability to:

- Withstand disruption
- Continue essential service
- Recover

Mechanisms include:

- Redundancy
- Failover
- Geographic diversity
- Replication

---

# Operational Redundancy

Example:

```text
Server A
   +
Server B
```

If A fails:

> B continues service.

---

# Erasure Coding

Erasure coding splits data into fragments with redundant encoding.

It can allow reconstruction even if some fragments are unavailable.

Commonly useful in:

- Distributed storage
- Resilient storage systems

Think:

> **Storage resilience with efficient redundancy**

---

# Survivability

Survivability asks:

> Can critical functions continue despite attack or failure?

Not every feature may remain available.

But:

> Mission-critical functions should survive where required.

---

# Denial of Service Resilience

Controls may include:

- Rate limiting
- Load balancing
- Redundancy
- Autoscaling
- Filtering
- Traffic scrubbing

---

# RTO and RPO

These concepts are useful for continuity planning.

## RTO — Recovery Time Objective

> How quickly must the service be restored?

Example:

```text
RTO = 2 hours
```

---

## RPO — Recovery Point Objective

> How much data loss is acceptable?

Example:

```text
RPO = 15 minutes
```

---

# RTO vs. RPO

```text
RTO
→ How long can we be down?

RPO
→ How much data can we lose?
```

---

# 7.13 Integrate Service-Level Objectives and Agreements

Operational security must support agreed service levels.

---

# SLO

**Service Level Objective**

An SLO defines a measurable performance or reliability target.

Example:

```text
Availability target = 99.95%
```

---

# SLA

**Service-Level Agreement**

An SLA is a formal agreement defining expected service levels.

It may include:

- Availability
- Performance
- Support
- Maintenance
- Incident response
- Qualified personnel
- Penalties/remedies

---

# SLO vs. SLA

```text
SLO
→ Specific measurable target.

SLA
→ Formal agreement containing service commitments.
```

---

# Example

SLO:

> 99.9% monthly uptime.

SLA:

> Provider commits to 99.9% uptime and provides service credits if the commitment is not met.

---

# Availability Calculation

Availability requirements may be expressed as percentages.

Example:

```text
99.9% availability
```

means some downtime is still permitted.

### Exam Point

A higher availability requirement generally requires:

- More redundancy
- Better monitoring
- Faster recovery
- Increased operational cost

---

# Maintenance Windows

SLAs may define:

- Planned maintenance periods
- Notification requirements
- Exceptions
- Emergency maintenance

---

# Qualified Personnel

Some service agreements may require:

> Appropriately trained or certified personnel.

This can support consistent and secure operation.

---

# High-Value Exam Distinctions

## Staging vs. Production

```text
Staging
→ Production-like validation environment.

Production
→ Real operational environment.
```

---

## Baseline vs. Current Configuration

```text
Baseline
→ Approved expected configuration.

Current Configuration
→ What is actually deployed.
```

Difference between them may represent:

> Configuration drift.

---

## Hash vs. Digital Signature

```text
Hash
→ Integrity indication.

Digital Signature
→ Integrity + authenticity.
```

---

## Credentials vs. Secrets vs. Keys

```text
Credentials
→ Prove identity/access.

Secrets
→ Sensitive confidential values.

Keys
→ Cryptographic material.
```

They overlap but require lifecycle management.

---

## Assessment vs. Approval to Operate

```text
Assessment
→ Evaluate controls and risk.

Approval
→ Authorized person accepts residual risk.
```

---

## Logs vs. Metrics vs. Traces

```text
Logs
→ Discrete events.

Metrics
→ Numerical measurements.

Traces
→ End-to-end transaction paths.
```

---

## IDS vs. SIEM

```text
IDS
→ Detect suspicious activity.

SIEM
→ Aggregate/correlate events from many sources.
```

---

## Triage vs. Root Cause Analysis

```text
Triage
→ What happened and how urgent is it?

Root Cause Analysis
→ Why did it happen?
```

---

## Patch Management vs. Vulnerability Management

```text
Patch Management
→ Manage updates.

Vulnerability Management
→ Manage weaknesses and risk.
```

---

## CVE vs. CVSS

```text
CVE
→ Vulnerability identifier.

CVSS
→ Technical severity score.
```

---

## WAF vs. RASP

```text
WAF
→ Protect web traffic externally.

RASP
→ Protect inside/near application runtime.
```

---

## ASLR vs. DEP

```text
ASLR
→ Randomize addresses.

DEP
→ Prevent executing data memory.
```

---

## Backup vs. Redundancy

```text
Backup
→ Recover data.

Redundancy
→ Maintain service.
```

---

## DRP vs. BCP

```text
DRP
→ Recover technology.

BCP
→ Continue business operations.
```

---

## RTO vs. RPO

```text
RTO
→ Recovery time.

RPO
→ Acceptable data loss.
```

---

## SLO vs. SLA

```text
SLO
→ Measurable target.

SLA
→ Formal service agreement.
```

---

# Common CSSLP Domain 7 Exam Traps

## Trap 1 — Test Configuration Equals Production Configuration

Wrong:

> It worked securely in QA, so production requires no additional review.

Better:

> Perform operational risk and configuration analysis for the actual deployment environment.

---

## Trap 2 — Development Credentials in Production

Wrong:

> Reuse development API keys because deployment is easier.

Better:

> Use environment-specific credentials and secrets.

---

## Trap 3 — Secrets in Git

Wrong:

```text
DATABASE_PASSWORD=ProductionSecret
```

committed to version control.

Better:

> Use controlled secrets management.

---

## Trap 4 — Hash Proves Publisher Identity

Wrong:

> Matching hash proves who released the software.

Better:

> A hash primarily establishes integrity; a trusted digital signature adds authenticity.

---

## Trap 5 — Developers Accept Production Risk

Wrong:

> Developer decides an unresolved critical issue is acceptable.

Better:

> Appropriate risk owner / authorizing authority makes the decision.

---

## Trap 6 — SIEM Prevents Every Attack

Wrong:

> Deploying a SIEM means attacks cannot occur.

Better:

> SIEM supports monitoring, correlation, detection, and response.

---

## Trap 7 — Patch Immediately Without Testing

Wrong:

> Every patch should be pushed immediately to every production server without validation.

Better:

> Balance urgency with testing and controlled deployment, using emergency processes when necessary.

---

## Trap 8 — Patch Management Equals Vulnerability Management

Wrong:

> If all vendor patches are installed, vulnerability management is complete.

Better:

> Vulnerabilities may require configuration changes, compensating controls, upgrades, or other remediation.

---

## Trap 9 — WAF Fixes Vulnerable Code

Wrong:

> SQL injection no longer needs remediation because WAF blocks known payloads.

Better:

> Fix the underlying vulnerability; use WAF as defense in depth.

---

## Trap 10 — Backup Equals Disaster Recovery

Wrong:

> We have backups, therefore our DR plan is complete.

Better:

> Recovery requires procedures, infrastructure, responsibilities, testing, and restoration capability.

---

## Trap 11 — Backup Equals High Availability

Wrong:

> Daily backups prevent downtime.

Better:

> Backups help recovery; redundancy/failover supports availability.

---

## Trap 12 — BCP Equals DRP

Wrong:

> Restoring the database is the entire business continuity plan.

Better:

> DRP is technology recovery; BCP addresses continued critical business operations.

---

## Trap 13 — Higher CVSS Always Means Higher Operational Risk

Wrong:

> Fix the highest CVSS finding first regardless of exposure.

Better:

> Combine severity with exploitability, asset value, exposure, and business impact.

---

## Trap 14 — Production Monitoring Is Only Logs

Wrong:

> Logging alone provides complete observability.

Better:

> Logs, metrics, traces, events, and telemetry provide complementary visibility.

---

## Trap 15 — Incident Remediation Is Enough

Wrong:

> Patch the compromised server and close the incident.

Better:

> Perform root cause analysis and lessons learned to prevent recurrence.

---

# Scenario Recognition Examples

## Scenario 1

Two production servers originally had identical secure settings.

Six months later, one has debug mode enabled and several unnecessary ports open.

Think:

> **Configuration drift**

---

## Scenario 2

A deployment pipeline retrieves a production database password directly from source code.

Think:

> **Secrets-management failure**

---

## Scenario 3

A company wants users to verify that a downloaded executable came from the vendor and was not modified.

Think:

> **Code signing**

---

## Scenario 4

Security assessment finds three medium risks before production.

Who decides whether the residual risk is acceptable?

Think:

> **Appropriate risk owner / authorizing authority**

---

## Scenario 5

Security teams collect:

- Identity logs
- Firewall events
- Application security events

and want central correlation.

Think:

> **SIEM**

---

## Scenario 6

A security alert may indicate account compromise.

The team first determines whether it is real and how severe it is.

Think:

> **Incident triage**

---

## Scenario 7

After containing malware, investigators preserve disk images and hashes.

Think:

> **Digital forensics / evidence integrity**

---

## Scenario 8

An incident was caused by stolen credentials.

Passwords are reset immediately.

Think:

> **Remediation**

The team later investigates why MFA was disabled.

Think:

> **Root cause analysis**

---

## Scenario 9

A vendor releases a critical patch for an Internet-facing component being actively exploited.

Think:

> **Risk-based emergency patch management**

---

## Scenario 10

An organization tracks hundreds of CVEs, validates whether systems are affected, prioritizes them, remediates, and verifies closure.

Think:

> **Vulnerability management**

---

## Scenario 11

A security control sits in front of a web application and filters malicious HTTP requests.

Think:

> **WAF**

---

## Scenario 12

Security protection runs inside the application and blocks suspicious runtime behavior.

Think:

> **RASP**

---

## Scenario 13

An exploit relies on knowing where libraries are loaded in memory.

The OS randomizes their location.

Think:

> **ASLR**

---

## Scenario 14

A system blocks code execution from memory regions designated for data.

Think:

> **DEP**

---

## Scenario 15

The business can tolerate two hours of application downtime but no more.

Think:

> **RTO**

---

## Scenario 16

The organization can tolerate losing at most 15 minutes of transaction data.

Think:

> **RPO**

---

## Scenario 17

The data center fails, and IT activates systems at another site.

Think:

> **Disaster Recovery**

---

## Scenario 18

While technology is being restored, the company activates manual procedures so critical customer services continue.

Think:

> **Business Continuity**

---

# Operational Security Workflow

```text
Assess Operational Risk
        ↓
Establish Baseline
        ↓
Secure Release
        ↓
Manage Secrets / Keys
        ↓
Harden Deployment
        ↓
Approve Operation
        ↓
Monitor Continuously
        ↓
Respond to Incidents
        ↓
Patch / Remediate
        ↓
Verify
        ↓
Maintain Continuity
```

---

# Incident Response Quick Flow

```text
Alert
 ↓
Triage
 ↓
Contain
 ↓
Preserve Evidence
 ↓
Investigate
 ↓
Remediate
 ↓
Recover
 ↓
Root Cause Analysis
 ↓
Lessons Learned
```

---

# Vulnerability Management Quick Flow

```text
Discover
 ↓
Validate
 ↓
Determine Exposure
 ↓
Score / Prioritize
 ↓
Remediate
 ↓
Retest
 ↓
Close
```

---

# Secure Deployment Checklist

```text
[ ] Artifact signature/hash verified?

[ ] Approved version deployed?

[ ] Default accounts removed?

[ ] Debug features disabled?

[ ] Least privilege configured?

[ ] Unnecessary services disabled?

[ ] Firewall rules restricted?

[ ] Secrets stored securely?

[ ] Certificates valid?

[ ] Security baseline applied?

[ ] Logging enabled?

[ ] Monitoring integrated?

[ ] Backup configured?

[ ] Restoration tested?

[ ] Patch level verified?

[ ] Production documentation updated?

[ ] Security approval obtained?
```

---

# Continuous Monitoring Checklist

```text
[ ] Authentication events collected?

[ ] Authorization failures collected?

[ ] Privileged activity monitored?

[ ] Application security events logged?

[ ] Infrastructure metrics collected?

[ ] Distributed traces available?

[ ] Threat intelligence incorporated?

[ ] Alerts prioritized?

[ ] SIEM correlation working?

[ ] Monitoring access restricted?

[ ] Privacy requirements respected?

[ ] Retention requirements defined?
```

---

# Fast Memory Sheet

```text
OPERATIONAL RISK ANALYSIS
→ What new risks exist in production?

BASELINE
→ Approved secure configuration.

CONFIGURATION DRIFT
→ Current state deviates from baseline.

VERSION CONTROL
→ Track changes and versions.

SECURE CI/CD
→ Protect the automated release pipeline.

CODE SIGNING
→ Verify artifact authenticity and integrity.

SECRETS MANAGEMENT
→ Store and rotate sensitive credentials safely.

HARDENING
→ Remove unnecessary attack surface.

IAAC
→ Repeatable infrastructure configuration.

APPROVAL TO OPERATE
→ Authorized acceptance of residual risk.

OBSERVABILITY
→ Understand system behavior through telemetry.

SIEM
→ Central security-event correlation.

TRIAGE
→ Determine incident urgency and scope.

FORENSICS
→ Preserve and analyze evidence.

REMEDIATION
→ Fix the immediate issue.

ROOT CAUSE
→ Determine why it happened.

PATCH MANAGEMENT
→ Evaluate, test, deploy, verify updates.

VULNERABILITY MANAGEMENT
→ Manage security weaknesses end-to-end.

CVE
→ Specific vulnerability identifier.

CVSS
→ Technical severity score.

RASP
→ Runtime protection inside/near application.

WAF
→ Filter malicious web traffic.

ASLR
→ Randomize memory locations.

DEP
→ Prevent execution from data regions.

BACKUP
→ Restore data.

DRP
→ Restore technology.

BCP
→ Continue business.

RTO
→ Maximum acceptable recovery time.

RPO
→ Maximum acceptable data loss.

SLO
→ Measurable service target.

SLA
→ Formal service commitment.
```

---

# Hardest Domain 7 Distinctions to Memorize

| Question Is Asking... | Likely Concept |
|---|---|
| Approved secure configuration? | **Baseline** |
| Server differs from approved configuration? | **Configuration Drift** |
| Verify publisher + artifact integrity? | **Code Signing** |
| Safely store API keys/passwords? | **Secrets Management** |
| Reduce unnecessary production exposure? | **Hardening** |
| Who accepts remaining production risk? | **Authorizing Authority / Risk Owner** |
| Correlate many security-event sources? | **SIEM** |
| Determine incident urgency? | **Triage** |
| Preserve evidence? | **Forensics** |
| Fix immediate compromise? | **Remediation** |
| Determine why compromise occurred? | **Root Cause Analysis** |
| Manage software updates? | **Patch Management** |
| Manage all weaknesses? | **Vulnerability Management** |
| Specific vulnerability identifier? | **CVE** |
| Technical severity score? | **CVSS** |
| Filter malicious HTTP traffic? | **WAF** |
| Protect application from within runtime? | **RASP** |
| Randomize memory layout? | **ASLR** |
| Stop executing data memory? | **DEP** |
| Restore lost information? | **Backup** |
| Restore IT after disaster? | **DRP** |
| Continue critical business activity? | **BCP** |
| How fast must service recover? | **RTO** |
| How much data loss is acceptable? | **RPO** |
| Specific reliability target? | **SLO** |
| Contractual service commitment? | **SLA** |

---

# CSSLP Domain 7 Exam Strategy

Domain 7 often tests the distinction between:

> **Preventing, detecting, responding, and recovering.**

Use this mental model:

```text
Prevent
→ Hardening, least privilege, patching

Detect
→ Logging, IDS, SIEM, monitoring

Respond
→ Triage, containment, remediation

Recover
→ Backup, DRP, BCP
```

---

# FIRST Exam Logic

When an incident occurs:

Do not immediately:

- Delete evidence
- Reinstall everything
- Publicly disclose details
- Start random remediation

First follow:

> The established incident response process.

Triage and containment decisions should be controlled.

---

# BEST Exam Logic

When two answers seem correct, ask:

> Which option most directly reduces the operational risk while preserving controlled change and appropriate risk ownership?

---

# Example

A critical vulnerability is actively exploited against an Internet-facing application.

Choices may include:

A. Wait until next quarterly maintenance period  
B. Immediately patch production with no testing or change process  
C. Use the emergency change process to assess, test appropriately, deploy rapidly, and verify  
D. Accept the risk because production changes are dangerous  

Best:

> **C**

It balances:

- Security urgency
- Change control
- Testing
- Verification

---

# Operational Controls vs. Root-Cause Fixes

Runtime controls can reduce immediate risk.

Example:

```text
Vulnerable application
+
WAF rule
```

This may be useful as a compensating control.

But:

> The underlying vulnerability should still be remediated when appropriate.

---

# Defense in Depth in Operations

A secure production system may combine:

```text
Secure Configuration
        +
Least Privilege
        +
Patching
        +
WAF / RASP
        +
Logging
        +
SIEM
        +
Backups
        +
Incident Response
```

No single operational control should be assumed infallible.

---

# Domain 7 Master Sequence

Memorize:

```text
Analyze Risk
    ↓
Baseline
    ↓
Release Securely
    ↓
Manage Secrets
    ↓
Harden
    ↓
Approve
    ↓
Monitor
    ↓
Detect
    ↓
Respond
    ↓
Patch
    ↓
Manage Vulnerabilities
    ↓
Protect Runtime
    ↓
Recover
    ↓
Maintain Service Levels
```

---

# Final Exam Rules

### Rule 1

> Production introduces risks that may not exist in development or testing.

### Rule 2

> Maintain a known secure baseline and detect configuration drift.

### Rule 3

> Protect CI/CD pipelines because they can modify trusted production artifacts.

### Rule 4

> Hashing supports integrity; digital signatures add authenticity.

### Rule 5

> Credentials, secrets, keys, and certificates require lifecycle management.

### Rule 6

> Secure installation includes hardening, least privilege, patching, and secure provisioning.

### Rule 7

> Residual risk must be accepted by the appropriate authority—not automatically by developers or testers.

### Rule 8

> Continuous monitoring should use relevant logs, events, metrics, traces, telemetry, and threat intelligence.

### Rule 9

> SIEM aggregates and correlates security events; it does not inherently prevent every attack.

### Rule 10

> Incident triage determines urgency; forensics preserves/analyzes evidence; remediation fixes the immediate issue; root cause analysis determines why it happened.

### Rule 11

> Patch management is narrower than vulnerability management.

### Rule 12

> CVE identifies a vulnerability; CVSS describes technical severity.

### Rule 13

> WAF and RASP provide runtime protection but should not replace root-cause remediation.

### Rule 14

> ASLR randomizes memory locations; DEP prevents execution from data regions.

### Rule 15

> Backups support recovery; redundancy supports availability.

### Rule 16

> DRP restores technology; BCP maintains critical business operations.

### Rule 17

> RTO describes acceptable recovery time; RPO describes acceptable data loss.

### Rule 18

> Test backups by restoring them.

### Rule 19

> SLOs are measurable targets; SLAs are formal service commitments.

### Rule 20

> Emergency security changes should use controlled emergency procedures, not bypass governance.

### Rule 21

> When two answers appear correct, prefer the answer that addresses risk through controlled operational processes and proper risk ownership.

---

## Disclaimer

This is an independent CSSLP study guide and is not an official ISC2 publication or a collection of official exam questions.
