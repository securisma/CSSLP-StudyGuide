# CSSLP Domain 2 Study Guide
## Secure Software Lifecycle Management

This study guide focuses on recognizing the concepts tested in **CSSLP Domain 2: Secure Software Lifecycle Management**, especially in scenario-based questions using terms such as **BEST**, **MOST appropriate**, **PRIMARY**, and **FIRST**.

---

# Domain 2 Overview

Domain 2 focuses on managing security throughout the software lifecycle rather than implementing individual technical controls.

The major areas are:

1. Security within software development methodologies
2. Security standards
3. Security strategy and roadmaps
4. Security documentation
5. Security metrics
6. Application decommissioning
7. Security reporting
8. Integrated risk management
9. Secure operational practices

A useful way to think about Domain 2 is:

```text
Plan security
      ↓
Integrate security into development
      ↓
Measure security
      ↓
Report security
      ↓
Manage risk
      ↓
Operate securely
      ↓
Retire securely
```

---

# Fast Exam Recognition Table

| If You See... | Think... |
|---|---|
| Security activities integrated into sprints | **Secure Agile Development** |
| Security phase gates between lifecycle stages | **Waterfall / Control Gates** |
| Organization needs consistent security practices | **Security Standards** |
| Long-term security improvement plan | **Security Strategy / Roadmap** |
| Security requirements must be recorded and maintained | **Security Documentation** |
| Measuring remediation time or security performance | **Security Metrics** |
| A vulnerability exceeds acceptable release risk | **Break/Build Criteria** |
| Product is being retired | **Decommissioning / EOL** |
| Credentials remain after an application is retired | **Incomplete Decommissioning** |
| Old customer data must be destroyed | **Data Disposition** |
| Executives need security status | **Security Reporting / Dashboard** |
| Developers need feedback from security results | **Feedback Loop** |
| Determine likelihood and impact | **Risk Analysis / Assessment** |
| Decide what to do about risk | **Risk Management / Treatment** |
| Vulnerability severity versus financial impact | **Technical vs. Business Risk** |
| Unauthorized production change | **Change Management** |
| Security incident occurs | **Incident Response Plan** |
| Confirm product meets specifications | **Verification** |
| Confirm product meets intended use | **Validation** |
| Evaluate controls before approving operation | **Assessment and Authorization (A&A)** |

---

# 2.1 Manage Security Within a Software Development Methodology

Security should be integrated into whatever development methodology the organization uses.

The CSSLP exam may contrast methodologies such as:

- Agile
- Waterfall
- Hybrid approaches

The important principle is:

> **Security should be integrated into the development process, not added only at the end.**

---

# Agile and Security

Agile development typically works through short iterations or sprints.

Security activities should therefore occur continuously.

Examples:

```text
Sprint Planning
      ↓
Security Requirements
      ↓
Development
      ↓
Security Testing
      ↓
Review
      ↓
Feedback
      ↓
Next Sprint
```

Security activities may include:

- Security user stories
- Abuse cases
- Threat modeling
- Secure coding
- Code review
- Automated security testing
- Security acceptance criteria

### Exam Clue

If a question says:

> The organization uses Agile. When should security testing occur?

The best answer is generally:

> **Throughout development and within the iterative process.**

Not:

> Perform one security assessment before production deployment.

---

# Waterfall and Security

Waterfall follows more sequential development phases.

Example:

```text
Requirements
    ↓
Design
    ↓
Implementation
    ↓
Testing
    ↓
Deployment
```

Security should be incorporated into each stage.

Security checkpoints may occur between phases.

Example:

```text
Requirements
    ↓
Security Gate
    ↓
Design
    ↓
Security Gate
    ↓
Implementation
```

### Important Exam Concept

Waterfall does **not** mean:

> Security testing happens only at the end.

Security should still be considered throughout the lifecycle.

---

# Agile vs. Waterfall

| Agile | Waterfall |
|---|---|
| Iterative | Sequential |
| Frequent releases | Larger lifecycle stages |
| Continuous security activities | Security activities associated with lifecycle phases |
| Rapid feedback | Formal phase reviews often common |
| Security integrated into sprints | Security integrated into lifecycle stages |

### Exam Rule

The development methodology changes **how security is integrated**, not whether security is required.

---

# 2.2 Identify and Adopt Security Standards

Security standards help organizations establish consistent security expectations.

Examples include:

- Secure coding standards
- Authentication standards
- Cryptographic standards
- Logging standards
- Development standards
- Security testing standards

A standard provides repeatability.

Without standards:

```text
Team A → Security Method A
Team B → Security Method B
Team C → Security Method C
```

With standards:

```text
Organization Security Standard
            ↓
Team A
Team B
Team C
```

---

# Policy vs. Standard vs. Procedure vs. Guideline

Although exam questions can vary in terminology, this distinction is useful.

## Policy

High-level management direction.

Example:

> All sensitive organizational information must be protected.

Think:

> **What must be done?**

---

## Standard

Mandatory specific requirements.

Example:

> Sensitive information must use approved cryptographic algorithms.

Think:

> **Mandatory rule.**

---

## Procedure

Step-by-step instructions.

Example:

```text
1. Request certificate
2. Validate identity
3. Install certificate
4. Test configuration
```

Think:

> **How to do it.**

---

## Guideline

Recommended practice.

Usually less mandatory than a standard.

Think:

> **Recommended approach.**

---

# Security Awareness

Adopting security standards also requires people to understand them.

Security awareness helps:

- Developers recognize security risks
- Management understand responsibilities
- Teams correctly apply standards
- Security become part of organizational culture

### Exam Trap

Simply publishing a secure coding standard may not be sufficient.

Developers may also require:

- Training
- Awareness
- Tools
- Feedback

---

# 2.3 Security Strategy and Roadmap

A security strategy defines the organization's long-term security direction.

A roadmap explains how the organization plans to reach that future state.

Think:

```text
Current State
      ↓
Security Initiatives
      ↓
Milestones
      ↓
Target State
```

---

# Strategy vs. Roadmap

## Strategy

Defines:

> **Where are we going and why?**

Example:

> Reduce software security risk by integrating security into all development teams.

---

## Roadmap

Defines:

> **How and when will we get there?**

Example:

```text
Q1 → Developer security training
Q2 → SAST deployment
Q3 → Threat modeling program
Q4 → Security maturity assessment
```

---

# Security Milestones and Checkpoints

Security milestones allow organizations to verify that required security activities have occurred.

Examples:

- Architecture security review
- Threat modeling completed
- Code review completed
- Security testing completed
- Production approval obtained

---

# Control Gates

A control gate is a decision point in the lifecycle.

Example:

```text
Development
     ↓
Security Assessment
     ↓
PASS?
 /       \
Yes       No
 ↓         ↓
Release   Remediate
```

A product should not advance when required security criteria have not been satisfied.

---

# Break/Build Criteria

Break/build criteria determine whether development or deployment should continue.

Example:

```text
Critical vulnerability detected
          ↓
Build fails
          ↓
Fix vulnerability
          ↓
Re-test
```

Examples of possible release criteria:

- No critical vulnerabilities
- Required security tests completed
- Code signing successful
- Security review approved
- Required controls implemented

### Exam Clue

If a question asks:

> What should determine whether insecure software proceeds toward release?

Think:

> **Security gates or break/build criteria.**

---

# 2.4 Security Documentation

Security documentation provides evidence and guidance throughout the lifecycle.

Examples include:

- Security plans
- Security requirements
- Architecture documentation
- Threat models
- Risk assessments
- Test plans
- Test results
- Security exceptions
- Operational procedures
- Incident response procedures

Good documentation should be:

- Accurate
- Maintained
- Version controlled
- Accessible to authorized personnel
- Updated when the system changes

---

# Why Documentation Matters

Documentation supports:

- Repeatability
- Accountability
- Auditing
- Compliance
- Knowledge transfer
- Risk management

### Exam Trap

Documentation is not something created once and forgotten.

When the system changes:

> **Security documentation should be updated accordingly.**

---

# 2.5 Security Metrics

Metrics allow organizations to measure security performance.

A useful security metric should be:

- Relevant
- Measurable
- Repeatable
- Actionable
- Understandable

Examples include:

- Number of critical vulnerabilities
- Average remediation time
- Percentage of builds passing security tests
- Number of security defects escaping into production
- Security training completion
- Vulnerability recurrence rate

---

# Average Remediation Time

This measures how long it takes to fix identified security issues.

Example:

```text
Vulnerability Found: March 1
Vulnerability Fixed: March 6

Remediation Time = 5 days
```

Shorter remediation times generally indicate more responsive vulnerability management.

---

# Criticality

Not every security issue has the same importance.

Criticality may consider:

- Impact
- Exposure
- Data sensitivity
- Business importance
- Exploitability

### Exam Point

Organizations should prioritize security work based on **risk**, not simply the number of vulnerabilities.

---

# KPI

**Key Performance Indicator**

A KPI measures performance against an objective.

Example:

> 95% of critical vulnerabilities must be remediated within seven days.

---

# OKR

**Objectives and Key Results**

An OKR combines a desired outcome with measurable results.

Example:

```text
Objective:
Improve software security response.

Key Result:
Reduce average critical vulnerability remediation
time from 14 days to 5 days.
```

---

# Metric vs. Target

Be careful with this distinction.

Metric:

```text
Average remediation time = 8 days
```

Target:

```text
Average remediation time must be ≤ 5 days
```

The metric tells you **what happened**.

The target tells you **what should happen**.

---

# Good Metrics vs. Vanity Metrics

A metric should support decision-making.

Weak metric:

> We ran 50,000 security scans.

That does not necessarily indicate better security.

Better metric:

> Critical vulnerability escape rate decreased by 60%.

### Exam Principle

Choose metrics that demonstrate **security outcomes**, not merely activity.

---

# 2.6 Decommission Applications

Security responsibilities continue when software reaches the end of its life.

Decommissioning means securely retiring an application.

It may include:

- Removing credentials
- Removing configurations
- Canceling licenses
- Disabling services
- Removing network access
- Archiving required information
- Destroying unnecessary information
- Updating dependencies
- Ending support agreements

---

# End of Life — EOL

End of Life means an application is no longer supported or operational.

An EOL plan should address:

```text
Application
    ↓
Dependencies
    ↓
Data
    ↓
Credentials
    ↓
Licenses
    ↓
Infrastructure
    ↓
Contracts / SLAs
```

---

# Credential Removal

A common decommissioning mistake is retiring an application but leaving:

- Service accounts
- API keys
- Certificates
- Cloud credentials
- Database credentials

These become **orphaned credentials**.

### Exam Clue

If an application is retired but credentials still work:

> The decommissioning process is incomplete.

---

# Configuration Removal

Security-related configuration may also need removal.

Examples:

- Firewall rules
- DNS entries
- IAM roles
- Load balancer rules
- Network routes
- Monitoring configuration

Unused configuration can expand the attack surface.

---

# License Cancellation

Applications may depend on commercial licenses or subscriptions.

Decommissioning should verify:

- Licenses are canceled
- Vendor access is removed
- Support agreements are terminated appropriately

---

# Archiving

Some application information may need to be retained.

Reasons include:

- Legal requirements
- Regulatory requirements
- Business requirements
- Audit requirements

Archiving does **not** mean keeping everything forever.

Retention requirements should determine what is preserved.

---

# Data Disposition

Data disposition determines what happens to information when the application is retired.

Possible actions:

```text
Retain
Archive
Transfer
Anonymize
Destroy
```

The correct action depends on:

- Legal requirements
- Regulatory requirements
- Business requirements
- Data classification
- Retention requirements

---

# Retention vs. Destruction

## Retention

Keep data for a required period.

## Destruction

Securely eliminate data when it is no longer required.

### Exam Rule

Do not keep data:

> **Just in case.**

Unnecessary retention creates:

- Privacy risk
- Legal risk
- Storage risk
- Security risk

---

# Dependencies During Decommissioning

Before removing an application, identify what depends on it.

Example:

```text
Application A
    ↓
Provides API
    ↓
Application B
```

Immediately deleting Application A may break Application B.

Therefore:

> **Identify dependencies before decommissioning.**

---

# 2.7 Security Reporting Mechanisms

Security information must reach the people who can act on it.

Reporting mechanisms may include:

- Reports
- Dashboards
- Alerts
- Management summaries
- Developer feedback
- Feedback loops

---

# Reports

Reports may contain:

- Vulnerability trends
- Security incidents
- Remediation performance
- Risk exposure
- Compliance status

---

# Dashboards

Dashboards provide summarized security information.

Example:

```text
Critical vulnerabilities: 4
High vulnerabilities: 17
Average remediation time: 6 days
Security gate pass rate: 92%
```

Different audiences need different information.

---

# Executive vs. Technical Reporting

## Executives Need

- Business risk
- Trends
- Financial impact
- Compliance status
- Major exposure
- Strategic decisions required

## Developers Need

- Vulnerability location
- Technical details
- Reproduction steps
- Recommended remediation

### Exam Rule

Security reporting should be:

> **Appropriate for the intended audience.**

---

# Feedback Loops

Security results should influence future development.

Example:

```text
Security Testing
      ↓
Vulnerability Found
      ↓
Developer Fix
      ↓
Root Cause Identified
      ↓
Coding Standard Updated
      ↓
Developer Training
```

This is much better than repeatedly fixing the same vulnerability.

---

# Metrics vs. Reporting

These are easy to confuse.

## Metrics

Measure something.

Example:

> Average vulnerability remediation time = 8 days.

## Reporting

Communicates the measurement.

Example:

> Management dashboard shows remediation time increased from 5 to 8 days.

### Memory Phrase

```text
Metrics = Measure
Reporting = Communicate
```

---

# 2.8 Integrated Risk Management

Risk management should be integrated into software lifecycle decisions.

Important areas include:

- Regulations
- Standards
- Guidelines
- Legal obligations
- Risk assessments
- Risk analysis
- Technical risk
- Business risk

---

# Risk

A useful conceptual model is:

```text
Risk = Likelihood × Impact
```

Real-world risk methodologies may be more complex, but CSSLP questions commonly revolve around:

> **How likely is something to happen, and how serious would the consequences be?**

---

# Threat, Vulnerability, and Risk

## Threat

Something capable of causing harm.

Example:

> Attacker

---

## Vulnerability

A weakness that could be exploited.

Example:

> SQL injection vulnerability

---

## Impact

The consequence if the vulnerability is exploited.

Example:

> Exposure of customer records

---

## Risk

The combination of likelihood and impact.

Example:

```text
Internet-facing SQL injection
        +
Sensitive financial data
        =
High business risk
```

---

# Risk Assessment vs. Risk Analysis vs. Risk Management

## Risk Assessment

Identifies and evaluates risk.

Think:

> **What risks do we have?**

---

## Risk Analysis

Examines factors such as:

- Likelihood
- Impact
- Threat
- Vulnerability
- Exposure

Think:

> **How serious is this risk?**

---

## Risk Management

Determines what to do about risk.

Think:

> **What are we going to do about it?**

---

# Risk Treatment

Common risk responses include:

```text
Avoid
Mitigate
Transfer
Accept
```

---

## Avoid

Stop the risky activity.

Example:

> Remove an unnecessary vulnerable service.

---

## Mitigate

Reduce likelihood or impact.

Example:

> Implement stronger access controls.

---

## Transfer

Shift some financial or operational risk.

Examples:

- Cyber insurance
- Contractual transfer
- Outsourcing under contract

### Important

Risk transfer rarely eliminates all responsibility.

---

## Accept

Management knowingly accepts the remaining risk.

### Critical Exam Rule

Risk acceptance should be performed by:

> **The person or business authority with appropriate risk ownership and authority.**

A developer should not independently accept major organizational risk merely because fixing the issue is difficult.

---

# Technical Risk vs. Business Risk

This is extremely important for CSSLP.

## Technical Risk

Focuses on technical severity.

Examples:

- Remote code execution
- Privilege escalation
- SQL injection
- Weak encryption
- Buffer overflow

---

## Business Risk

Focuses on organizational consequences.

Examples:

- Financial loss
- Regulatory penalties
- Loss of customers
- Reputation damage
- Operational outage
- Legal liability

---

# Example

Two systems contain the same critical vulnerability.

### System A

Internal development test server containing synthetic data.

### System B

Internet-facing payment system processing millions of euros.

The technical vulnerability may be identical.

But the **business risk is much higher for System B**.

### Exam Rule

Technical severity alone does not determine organizational risk.

Think:

```text
Technical Severity
        +
Business Context
        =
Actual Risk Priority
```

---

# Standards and Frameworks

Domain 2 includes awareness of security standards, frameworks, and guidance such as:

- ISO
- NIST
- PCI
- OWASP
- SAFECode
- OWASP SAMM
- BSIMM

You typically need to understand **why organizations use them**, rather than memorize every control.

---

# NIST

NIST publishes cybersecurity standards, guidance, and frameworks.

In software security, NIST guidance can help organizations structure:

- Secure development
- Risk management
- Security controls
- Cybersecurity programs

---

# OWASP

OWASP provides application security guidance, tools, and community resources.

Examples include:

- OWASP Top 10
- OWASP ASVS
- OWASP SAMM

Think:

> **Application security guidance and maturity.**

---

# OWASP SAMM

**Software Assurance Maturity Model**

SAMM helps organizations evaluate and improve their software security practices.

Think:

> **Software security maturity improvement.**

---

# BSIMM

**Building Security In Maturity Model**

BSIMM examines software security initiatives and practices observed across organizations.

Think:

> **Software security maturity / benchmarking.**

---

# PCI

PCI security requirements are particularly relevant to payment card environments.

Think:

> **Payment card security.**

---

# SAFECode

SAFECode provides guidance related to secure software development practices.

Think:

> **Software assurance and secure development guidance.**

---

# Regulations vs. Standards vs. Guidelines

## Regulation

Legally enforceable requirement issued under governmental or regulatory authority.

Think:

> **Must comply when applicable.**

---

## Standard

Defined requirements or practices.

May be:

- Mandatory due to organizational policy
- Required by contract
- Required by an industry program

---

## Guideline

Recommended practice.

Usually provides more flexibility.

---

# Legal Considerations

Domain 2 includes legal concerns such as:

- Intellectual property
- Breach notification

---

# Intellectual Property

Intellectual property may include:

- Source code
- Algorithms
- Designs
- Documentation
- Trade secrets
- Copyrighted material
- Patents

Software lifecycle management should address ownership and protection of intellectual property.

---

# Breach Notification

Security incidents may trigger legal or regulatory notification requirements.

Organizations should understand:

- Who must be notified
- When notification is required
- Required notification timelines
- Jurisdiction-specific requirements

### Exam Trap

The development team should not independently decide whether to hide or disclose a breach.

The response should follow:

- Incident response procedures
- Legal requirements
- Regulatory requirements
- Organizational governance

---

# 2.9 Secure Operational Practices

Domain 2 also covers secure operational practices that support the software lifecycle.

These include:

- Change management
- Incident response
- Verification and validation
- Assessment and Authorization

---

# Change Management

Change management ensures changes are:

- Requested
- Evaluated
- Approved
- Tested
- Documented
- Implemented
- Reviewed

A basic process:

```text
Change Request
      ↓
Impact Assessment
      ↓
Approval
      ↓
Testing
      ↓
Implementation
      ↓
Verification
      ↓
Documentation
```

---

# Why Change Management Matters

Uncontrolled changes can introduce:

- Vulnerabilities
- Configuration errors
- Service outages
- Compliance violations
- Unauthorized functionality

---

# Emergency Changes

Emergency changes may require accelerated processes.

However:

> **Emergency does not mean uncontrolled.**

Emergency changes should still be:

- Authorized appropriately
- Documented
- Tested where practical
- Reviewed afterward

### Exam Trap

Do not select:

> Skip change management because the vulnerability is urgent.

Instead:

> Use the approved emergency change process.

---

# Incident Response Plan

An incident response plan defines how an organization responds to security incidents.

A commonly understood flow is:

```text
Preparation
    ↓
Detection / Identification
    ↓
Containment
    ↓
Eradication
    ↓
Recovery
    ↓
Lessons Learned
```

Exact terminology may vary between frameworks.

---

# Incident Response vs. Disaster Recovery

These are different.

## Incident Response

Focuses on:

> Security incidents.

Examples:

- Malware
- Data breach
- Account compromise

---

## Disaster Recovery

Focuses on:

> Restoring systems and operations following significant disruption.

### Memory Phrase

```text
Incident Response = Handle the security incident
Disaster Recovery = Restore operational capability
```

---

# Verification vs. Validation

This distinction is highly testable.

## Verification

Asks:

> **Did we build the product correctly?**

Verification compares the implementation against specifications and requirements.

Examples:

- Code review
- Requirements traceability
- Design review
- Static analysis

Memory phrase:

> **Are we building the product right?**

---

## Validation

Asks:

> **Did we build the right product?**

Validation determines whether the software satisfies its intended use and stakeholder needs.

Examples:

- Acceptance testing
- User validation
- Operational evaluation

Memory phrase:

> **Are we building the right product?**

---

# Verification vs. Validation Shortcut

```text
Verification
→ Built RIGHT?

Validation
→ Built the RIGHT thing?
```

This distinction is extremely useful for exam questions.

---

# Assessment and Authorization — A&A

Assessment and Authorization determines whether a system's security posture is acceptable for operation.

There are two important concepts.

## Assessment

Evaluate whether required controls:

- Exist
- Are correctly implemented
- Operate effectively

Think:

> **Evaluate the evidence.**

---

## Authorization

An authorized decision-maker determines whether the remaining risk is acceptable.

Think:

> **Accept the risk and approve operation.**

---

# Assessment vs. Authorization

```text
Assessment
     ↓
Evaluate controls and risk
     ↓
Authorization
     ↓
Management decision to operate
```

### Critical Exam Rule

The assessor provides information.

The authorizing authority:

> **Accepts or rejects the risk.**

These responsibilities should not be confused.

---

# High-Value Exam Distinctions

## Security Strategy vs. Roadmap

```text
Strategy = Where and why
Roadmap  = How and when
```

---

## Metric vs. Reporting

```text
Metric    = Measure
Reporting = Communicate
```

---

## Risk Analysis vs. Risk Management

```text
Risk Analysis   = How serious is the risk?
Risk Management = What will we do about it?
```

---

## Technical Risk vs. Business Risk

```text
Technical Risk = Technical severity
Business Risk  = Organizational consequence
```

---

## Verification vs. Validation

```text
Verification = Built right?
Validation   = Built the right thing?
```

---

## Assessment vs. Authorization

```text
Assessment    = Evaluate security
Authorization = Accept risk and approve operation
```

---

## Decommissioning vs. Data Destruction

```text
Decommissioning
= Retire the entire application securely

Data Destruction
= One activity that may occur during decommissioning
```

---

## Standard vs. Guideline

```text
Standard  = Required practice
Guideline = Recommended practice
```

---

# Common CSSLP Domain 2 Exam Traps

## Trap 1 — Security at the End

Wrong thinking:

> Development is complete, so now we perform security.

Better:

> Security should be integrated throughout the lifecycle.

---

## Trap 2 — Vulnerability Count Equals Risk

Wrong thinking:

> The application with more vulnerabilities always has more risk.

Better:

> Risk depends on likelihood, impact, exposure, and business context.

---

## Trap 3 — Developer Accepts Risk

Wrong thinking:

> The developer decides the vulnerability is acceptable.

Better:

> Risk should be accepted by the appropriate risk owner or authorized business authority.

---

## Trap 4 — Emergency Means Skip Process

Wrong thinking:

> Emergency patch means no change management.

Better:

> Use the approved emergency change-management process.

---

## Trap 5 — Retired Application Means Finished

Wrong thinking:

> Turn off the server and decommissioning is complete.

Better:

Also consider:

- Credentials
- Data
- Dependencies
- Licenses
- Firewall rules
- DNS
- Contracts
- Archives

---

## Trap 6 — More Metrics Means Better Security

Wrong thinking:

> Collect as many metrics as possible.

Better:

> Collect meaningful and actionable metrics tied to security objectives.

---

## Trap 7 — Technical Severity Equals Business Priority

Wrong thinking:

> CVSS or technical severity alone determines priority.

Better:

> Consider technical severity together with business impact and context.

---

## Trap 8 — Assessment Means Approval

Wrong thinking:

> The security assessor approves the system for production.

Better:

> The assessor evaluates. The authorized decision-maker accepts or rejects risk.

---

# Scenario Recognition Examples

## Scenario 1

A development organization discovers critical vulnerabilities two hours before release.

The release policy states that software containing unresolved critical vulnerabilities cannot enter production.

Think:

> **Break/build criteria or security control gate.**

---

## Scenario 2

A development team fixes vulnerabilities quickly, but the same vulnerabilities repeatedly appear in new products.

Best improvement:

> Establish a feedback loop and address the root cause through standards, training, or process changes.

---

## Scenario 3

A retired application has been shut down, but its database service account still has administrator access.

Think:

> **Incomplete decommissioning / credential removal.**

---

## Scenario 4

A critical vulnerability exists on two systems.

One system contains public information.

The other processes financial transactions.

Think:

> **Business risk determines prioritization.**

---

## Scenario 5

An assessor determines that several security controls are partially ineffective.

Who decides whether the system can operate with the remaining risk?

Think:

> **Authorized risk owner / authorizing authority.**

---

## Scenario 6

A developer applies an emergency production patch without approval because exploitation is occurring.

Even though the patch works, what lifecycle process was bypassed?

Think:

> **Change management.**

A better approach would have been the organization's emergency change process.

---

## Scenario 7

An organization measures:

> Average time to remediate critical vulnerabilities = 12 days.

That value is a:

> **Security metric.**

If management sees that value on a monthly dashboard:

> **Security reporting.**

---

# Fast Memory Sheet

```text
SECURITY + SDLC
→ Integrate security throughout development.

AGILE
→ Security continuously in iterations.

WATERFALL
→ Security throughout sequential lifecycle stages.

STANDARD
→ Mandatory consistent requirement.

STRATEGY
→ Where and why.

ROADMAP
→ How and when.

CONTROL GATE
→ Security decision point.

BREAK/BUILD CRITERIA
→ Determine whether development/release continues.

DOCUMENTATION
→ Record and maintain security information.

METRICS
→ Measure security.

REPORTING
→ Communicate security.

EOL
→ Securely retire applications.

DATA DISPOSITION
→ Retain, archive, transfer, or destroy data appropriately.

RISK ASSESSMENT
→ Identify and evaluate risk.

RISK ANALYSIS
→ Determine likelihood and impact.

RISK MANAGEMENT
→ Decide what to do about risk.

TECHNICAL RISK
→ Technical severity.

BUSINESS RISK
→ Organizational impact.

CHANGE MANAGEMENT
→ Control changes.

INCIDENT RESPONSE
→ Handle security incidents.

VERIFICATION
→ Did we build it right?

VALIDATION
→ Did we build the right thing?

ASSESSMENT
→ Evaluate controls and risk.

AUTHORIZATION
→ Management accepts risk and approves operation.
```

---

# Hardest Domain 2 Distinctions to Memorize

| Question Is Asking... | Likely Concept |
|---|---|
| How severe is the technical flaw? | **Technical Risk** |
| What could the flaw cost the organization? | **Business Risk** |
| What risks exist? | **Risk Assessment** |
| How likely/serious are they? | **Risk Analysis** |
| What should we do about them? | **Risk Management** |
| Did implementation match specifications? | **Verification** |
| Does the product satisfy real needs? | **Validation** |
| Were controls properly implemented? | **Assessment** |
| Can the system operate with remaining risk? | **Authorization** |
| How are we performing? | **Metrics** |
| How do we communicate performance? | **Reporting** |
| Where are we going? | **Strategy** |
| How will we get there? | **Roadmap** |
| Can this release continue? | **Control Gate / Break-Build Criteria** |
| What happens when software reaches EOL? | **Decommissioning** |

---

# CSSLP Domain 2 Exam Strategy

Domain 2 questions often test **management judgment** rather than purely technical knowledge.

When two answers appear correct, consider this order:

```text
1. Legal / regulatory obligations
2. Organizational governance and policy
3. Risk assessment
4. Appropriate management approval
5. Technical implementation
```

For example:

A developer discovers a serious vulnerability shortly before release.

The developer should generally **not independently decide to accept the risk**.

The appropriate process is closer to:

```text
Identify vulnerability
        ↓
Assess risk
        ↓
Report risk
        ↓
Apply established release criteria
        ↓
Escalate to appropriate authority
        ↓
Remediate or formally accept risk
```

---

# Final Exam Rules

When a Domain 2 question contains **BEST**, **MOST appropriate**, or **PRIMARY**, remember:

### Rule 1

> Security belongs throughout the software lifecycle.

### Rule 2

> Business risk matters more than technical severity alone.

### Rule 3

> Risk acceptance belongs to the appropriate risk owner or authorized authority.

### Rule 4

> Metrics should drive decisions, not merely generate numbers.

### Rule 5

> Security findings should create feedback loops that improve future development.

### Rule 6

> Decommissioning includes systems, data, credentials, dependencies, and supporting infrastructure.

### Rule 7

> Emergency changes should still follow an authorized emergency process.

### Rule 8

> Verification asks whether the product was built correctly; validation asks whether the correct product was built.

### Rule 9

> Assessment evaluates risk; authorization accepts risk.

### Rule 10

> When two answers seem correct, choose the one that best supports repeatable lifecycle governance and appropriate risk ownership.

---

## Disclaimer

This is an independent CSSLP study guide and is not an official ISC2 publication or a collection of official exam questions.
