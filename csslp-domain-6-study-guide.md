# CSSLP Domain 6 Study Guide
## Secure Software Testing

This study guide focuses on **CSSLP Domain 6: Secure Software Testing**.

Domain 6 is about proving that software security requirements and controls actually work—and discovering weaknesses before attackers do.

The major areas are:

1. Security testing strategy and planning
2. Security test cases
3. Documentation verification and validation
4. Undocumented functionality
5. Security implications of test results
6. Security error classification and tracking
7. Secure test data
8. Verification and validation testing

---

# Domain 6 Mental Model

Think of Domain 6 as:

```text
Requirements
     ↓
Security Test Strategy
     ↓
Test Cases
     ↓
Execute Tests
     ↓
Find Defects
     ↓
Analyze Risk
     ↓
Track / Remediate
     ↓
Regression Testing
     ↓
Verification & Validation
```

Domain 5 asks:

> Was the software implemented securely?

Domain 6 asks:

> Can we demonstrate that it behaves securely under normal, abnormal, and hostile conditions?

---

# Fast Exam Recognition Table

| If You See... | Think... |
|---|---|
| Overall approach to security testing | **Test Strategy / Plan** |
| Test business/security behavior | **Functional Security Testing** |
| Reliability, performance, scalability | **Nonfunctional Testing** |
| Tester knows internal implementation | **Known Environment Testing** |
| Tester has little/no internal knowledge | **Unknown Environment Testing** |
| Validate business/customer expectations | **Acceptance Testing** |
| Automated running-app vulnerability testing | **DAST** |
| Testing observes app internally while running | **IAST** |
| Human actively attempts exploitation | **Penetration Testing** |
| Random/malformed inputs | **Fuzzing** |
| Generate new unexpected inputs | **Generation-Based Fuzzing** |
| Modify valid existing inputs | **Mutation-Based Fuzzing** |
| Simulate production behavior | **Simulation** |
| Deliberately inject component failures | **Fault Injection** |
| Push system beyond normal limits | **Stress Testing** |
| Intentionally cause failures to test resilience | **Break Testing** |
| Validate RNG quality | **Entropy / PRNG Testing** |
| Test one small function/module | **Unit Testing** |
| How much code was executed by tests | **Code Coverage** |
| Ensure a fix did not break old behavior | **Regression Testing** |
| Test components working together | **Integration Testing** |
| Test throughout CI/CD | **Continuous Testing** |
| Test malicious behavior scenarios | **Misuse / Abuse Testing** |
| User guide contradicts actual product | **Documentation Validation** |
| Hidden API/account/function discovered | **Undocumented Functionality** |
| Finding determines whether build proceeds | **Break/Build Criteria** |
| Track defect status and remediation | **Bug Tracking** |
| Score technical vulnerability severity | **CVSS** |
| Fake but realistic test information | **Synthetic Test Data** |
| Remove identifying information from test data | **Anonymization** |
| Replace real values with tokens | **Tokenization** |
| Independent group confirms requirements | **Independent V&V** |

---

# 6.1 Develop Security Testing Strategy and Plan

A security test strategy defines:

> **What will be tested, how it will be tested, when it will be tested, and who is responsible.**

A strong strategy considers:

- Security requirements
- Application risk
- Architecture
- Threat model
- Compliance requirements
- Test environment
- Test techniques
- Required tools
- Entry and exit criteria
- Defect handling
- Retesting

---

# Risk-Based Testing

Not every component requires the same testing depth.

Testing effort should be prioritized according to:

```text
Risk
+
Criticality
+
Exposure
+
Threat Model
+
Data Sensitivity
```

Example:

```text
Public marketing page
        ≠
Internet-facing payment API
```

The payment API should generally receive more intensive security testing.

### Exam Rule

> Testing effort should be proportional to risk, not distributed equally without regard to criticality.

---

# Security Testing Standards

Testing strategies may rely on recognized standards and methodologies.

Examples include:

- ISO standards
- Open Source Security Testing Methodology Manual (OSSTMM)
- Software Engineering Institute (SEI) guidance
- Organizational testing standards

The purpose is to provide:

- Repeatability
- Consistency
- Coverage
- Defined methodology

---

# Functional Security Testing

Functional testing verifies whether required security functionality behaves correctly.

Examples:

- Does MFA activate when required?
- Does account lockout work?
- Does authorization block unauthorized users?
- Are security events logged?
- Does session termination work?

Think:

> **Does the security function do what the requirement says?**

---

# Example

Requirement:

> Users without the Finance Approver role cannot approve payments.

Functional security test:

```text
Login as standard user
        ↓
Attempt approval
        ↓
Expected result:
ACCESS DENIED
```

---

# Nonfunctional Security Testing

Nonfunctional testing evaluates security-related qualities such as:

- Reliability
- Performance
- Scalability
- Availability
- Resilience

Example:

> Can the authentication system maintain acceptable performance with 100,000 concurrent users?

This is not simply checking whether authentication works.

It is checking:

> How the system behaves under operational conditions.

---

# Functional vs. Nonfunctional

```text
Functional
→ Does the security feature work?

Nonfunctional
→ How well does it operate under constraints?
```

---

# Known vs. Unknown Environment Testing

These concepts are often comparable to white-box and black-box perspectives.

## Known Environment

Tester has substantial information about the system.

May include:

- Source code
- Architecture
- Credentials
- Network diagrams
- Internal documentation

Advantages:

- Deeper coverage
- More efficient testing
- Internal logic can be targeted

---

## Unknown Environment

Tester receives limited internal information.

The tester approaches the application more like an external attacker.

Advantages:

- Simulates attacker perspective
- Tests externally exposed behavior

---

# Known vs. Unknown Shortcut

```text
Known Environment
→ Tester knows the internals.

Unknown Environment
→ Tester sees primarily what an attacker sees.
```

---

# Exam Trap

Unknown-environment testing is not automatically better.

Known-environment testing may achieve:

> Greater depth and coverage.

The best approach depends on the objective.

---

# Acceptance Testing

Acceptance testing determines whether software satisfies agreed requirements and stakeholder expectations.

It may include:

- Security acceptance criteria
- Business requirements
- Operational requirements
- Compliance requirements

Think:

> **Is the software acceptable for intended use?**

---

# Test Environment

Security testing should occur in a controlled environment representative of production.

Consider:

- Operating systems
- Network topology
- Databases
- Authentication systems
- External services
- Security controls

---

# Production-Like Environment

A test environment that differs greatly from production may hide security problems.

Example:

```text
Testing:
Single server

Production:
Load balancer + 20 servers + shared session store
```

Certain session or concurrency defects may appear only in production architecture.

### Exam Rule

> The environment should be sufficiently representative of production for the test objective.

---

# Interoperability Testing

Interoperability testing evaluates how systems interact.

Examples:

- Application ↔ Identity provider
- Application ↔ Payment gateway
- Service ↔ API
- Application ↔ Database

Security defects often occur at:

> **Integration and trust boundaries.**

---

# Test Harness

A test harness provides controlled tooling or simulated components for testing.

It may:

- Generate inputs
- Simulate dependencies
- Capture outputs
- Automate tests
- Validate expected results

Example:

```text
Test Harness
     ↓
Send malformed API calls
     ↓
Application
     ↓
Capture response
```

---

# Security Researcher Outreach

Organizations may allow external researchers to report vulnerabilities.

Examples include:

- Vulnerability disclosure programs
- Bug bounty programs

---

# Bug Bounty

A bug bounty rewards security researchers for responsibly reporting vulnerabilities.

Benefits:

- Broader researcher participation
- Real-world attack perspectives
- Additional testing coverage

### Exam Trap

A bug bounty does not replace:

- Secure development
- Internal testing
- Code review
- Penetration testing

It is an additional layer.

---

# 6.2 Develop Security Test Cases

Security test cases should derive from:

- Security requirements
- Threat models
- Misuse cases
- Abuse cases
- Architecture
- Known weakness classes
- Compliance requirements

---

# Positive vs. Negative Testing

## Positive Testing

Tests expected valid behavior.

Example:

> Authorized manager successfully approves payment.

---

## Negative Testing

Tests invalid or malicious behavior.

Example:

> Unauthorized user attempts payment approval.

Security testing relies heavily on:

> **Negative testing**

because attackers deliberately violate expected assumptions.

---

# Attack Surface Validation

Attack surface validation identifies and tests exposed interfaces.

Examples:

- APIs
- Ports
- Web endpoints
- Upload mechanisms
- Administrative consoles
- Service interfaces

Questions:

```text
Should this interface exist?
        ↓
Should it be externally reachable?
        ↓
Does it require authentication?
        ↓
Is authorization enforced?
```

---

# Attack Surface Validation Goal

Verify that:

- Required interfaces exist
- Unnecessary interfaces are disabled
- Exposed interfaces have appropriate controls

---

# DAST

**Dynamic Application Security Testing**

DAST examines a running application from the outside.

It interacts with the application through:

- HTTP
- APIs
- Network interfaces
- Inputs

Think:

> **Test the running application.**

---

# DAST Common Findings

DAST may detect:

- Injection
- XSS
- Authentication problems
- Server misconfiguration
- Exposed endpoints
- Runtime behavior issues

---

# DAST Strengths

- Tests actual runtime behavior
- Does not require source code
- Can identify exploitable behavior

---

# DAST Limitations

- Limited visibility into internal code paths
- May have difficulty locating exact source of defect
- Can miss unexecuted functionality

---

# IAST

**Interactive Application Security Testing**

IAST examines the application while it runs but has visibility inside the application.

Conceptually:

```text
Test Request
     ↓
Running Application
     +
Instrumentation
     ↓
Observe data flow internally
```

Think:

> **Runtime testing + internal visibility**

---

# DAST vs. IAST

```text
DAST
→ Observe running application externally.

IAST
→ Observe running application with internal instrumentation.
```

---

# SAST vs. DAST vs. IAST

Important cross-domain distinction:

```text
SAST
→ Analyze code without running it.

DAST
→ Attack/test running application externally.

IAST
→ Analyze running application with internal visibility.
```

---

# Penetration Testing

Penetration testing actively attempts to exploit security weaknesses.

A typical flow:

```text
Reconnaissance
      ↓
Enumeration
      ↓
Vulnerability Identification
      ↓
Exploitation
      ↓
Impact Evaluation
      ↓
Reporting
```

---

# Purpose of Penetration Testing

Pen testing can demonstrate:

> **Whether a vulnerability is actually exploitable and what impact exploitation may have.**

---

# Vulnerability Scan vs. Penetration Test

```text
Vulnerability Scan
→ Identify possible weaknesses.

Penetration Test
→ Attempt to exploit them.
```

---

# Exam Trap

A vulnerability scan reporting a weakness does not automatically prove:

> The vulnerability is exploitable.

Penetration testing can provide stronger evidence of exploitability.

---

# Penetration Test Scope

Scope should define:

- Systems included
- Systems excluded
- Test period
- Allowed techniques
- Data handling
- Contacts
- Stop conditions

This protects:

- Business operations
- Legal authorization
- Safety

---

# Fuzzing

Fuzzing sends unexpected or malformed input to software to discover defects.

Examples:

- Oversized input
- Invalid format
- Unexpected characters
- Corrupt files
- Invalid protocol messages

---

# Generation-Based Fuzzing

Creates test input based on knowledge of expected structure or protocol.

Example:

```text
Protocol Specification
       ↓
Generate unusual valid/invalid messages
```

---

# Mutation-Based Fuzzing

Starts with valid input and modifies it.

Example:

```text
Valid PDF
   ↓
Change bytes / fields
   ↓
Malformed PDF
```

---

# Generation vs. Mutation Fuzzing

```text
Generation-Based
→ Build test inputs from a model/specification.

Mutation-Based
→ Modify existing valid inputs.
```

---

# Why Fuzzing Works

Developers often test:

> Expected inputs.

Attackers use:

> Unexpected inputs.

Fuzzing specifically explores those unexpected states.

---

# Simulation

Simulation recreates expected operating conditions without requiring full live production activity.

Examples:

- Synthetic transactions
- Production-like traffic
- Simulated network conditions
- Simulated external services

---

# Production Data Simulation

Prefer realistic but protected test data.

Example:

```text
Realistic transaction patterns
+
Synthetic customer identities
```

This can provide representative testing while reducing privacy risk.

---

# Failure Testing

Security testing should intentionally examine failure behavior.

Examples include:

- Fault injection
- Stress testing
- Break testing

---

# Fault Injection

Fault injection deliberately causes component failures.

Examples:

- Network unavailable
- Database connection fails
- Disk full
- Authentication service unavailable
- Memory allocation failure

Purpose:

> Verify that the system handles failures securely and resiliently.

---

# Example

```text
Authorization server unavailable
        ↓
Expected:
Deny privileged request safely
```

If instead:

```text
Authorization server unavailable
        ↓
Allow everyone
```

the software fails insecurely.

---

# Stress Testing

Stress testing pushes the system beyond expected operating capacity.

Examples:

- Extreme concurrent users
- Large files
- High request volume
- High transaction rate

Purpose:

> Determine behavior near or beyond operational limits.

---

# Stress vs. Load Testing

Conceptually:

```text
Load Testing
→ Expected/high normal workload.

Stress Testing
→ Push beyond normal capacity.
```

---

# Break Testing

Break testing deliberately drives the system toward failure to understand:

- Failure points
- Recovery behavior
- Security state during failure

Think:

> **What happens when the software breaks?**

---

# Fault Injection vs. Stress Testing

```text
Fault Injection
→ Deliberately fail a component.

Stress Testing
→ Overload the system.
```

---

# Cryptographic Validation

Cryptographic functionality requires specific testing.

Examples:

- Algorithm implementation
- Random-number generation
- Entropy quality
- Key handling
- Encryption/decryption behavior
- Signature validation

---

# PRNG

**Pseudorandom Number Generator**

Security-sensitive uses may include:

- Session identifiers
- Cryptographic keys
- Nonces
- Tokens

A predictable PRNG can undermine security.

---

# Entropy

Entropy represents unpredictability/randomness.

Higher-quality cryptographic randomness should be:

> Difficult for attackers to predict.

---

# Exam Clue

If attackers can predict:

- Session identifiers
- Keys
- Reset tokens

Think:

> **Insufficient randomness / entropy**

---

# Unit Testing

Unit tests examine small pieces of software independently.

Examples:

- Function
- Method
- Class
- Module

Security unit tests may verify:

- Authorization decision
- Validation function
- Encryption wrapper
- Input parser

---

# Unit Test Example

```text
Function:
isAuthorized(user, resource)

Test:
StandardUser + AdminResource

Expected:
false
```

---

# Code Coverage

Code coverage measures how much code was executed during testing.

Possible measurements include:

- Statement coverage
- Branch coverage
- Function coverage
- Path coverage

---

# Critical Exam Rule

> High code coverage does not prove security.

Example:

```text
100% statement coverage
```

can still fail to test:

- Malicious inputs
- Security requirements
- Complex attack paths

Coverage tells you:

> What was executed.

Not:

> Whether it was tested securely.

---

# Branch Coverage

Branch coverage evaluates whether different decision outcomes were executed.

Example:

```text
if user.isAdmin:
    allow()
else:
    deny()
```

Testing only administrators misses:

> The deny branch.

---

# Regression Testing

Regression testing verifies that changes have not broken previously working behavior.

Security regression testing is especially important after:

- Vulnerability fixes
- Patches
- Dependency upgrades
- Architecture changes

---

# Security Regression Example

A developer fixes an authorization bypass.

Regression test:

```text
Attempt old bypass technique
        ↓
Expected:
Denied
```

This test should remain in the suite to prevent the defect from returning.

---

# Regression Testing Memory Phrase

> **Make sure the old bug stays dead.**

---

# Integration Testing

Integration testing examines interaction between components.

Examples:

- Application ↔ Database
- Service ↔ Identity provider
- API ↔ Payment processor

Security issues may arise because components make different assumptions.

---

# Integration Example

Service A assumes:

> Every request from Service B is authenticated.

Service B assumes:

> Service A performs authentication.

Result:

> Nobody authenticates.

Integration testing should expose this assumption failure.

---

# Continuous Testing

Continuous testing integrates testing into:

- CI
- CD
- DevSecOps pipelines

Example:

```text
Commit
  ↓
Build
  ↓
Unit Tests
  ↓
SAST
  ↓
Integration Tests
  ↓
DAST
  ↓
Security Gate
```

Goal:

> Discover security problems quickly and repeatedly.

---

# Continuous Testing Advantage

Earlier feedback generally means:

- Cheaper remediation
- Faster developer feedback
- Reduced chance of insecure release

---

# Misuse and Abuse Test Cases

Domain 3 defines misuse and abuse scenarios.

Domain 6 turns those scenarios into:

> Actual security tests.

---

# Example

Abuse case:

> User repeatedly resets another user's password.

Test case:

```text
Send 1,000 password reset requests
        ↓
Verify:
Rate limiting
Account protection
Logging
No account takeover
```

---

# 6.3 Verify and Validate Documentation

Security testing also includes documentation.

Examples:

- Installation instructions
- Setup instructions
- Error messages
- User guides
- Administrator guides
- Release notes

---

# Why Documentation Matters

Incorrect documentation can cause insecure deployment.

Example:

Documentation says:

> Disable TLS certificate verification to connect successfully.

Even if the application code is secure:

> The deployment instructions create insecurity.

---

# Installation and Setup Instructions

Verify that instructions produce a secure configuration.

Check:

- Default credentials
- Required permissions
- Secure protocols
- Network exposure
- Certificate configuration
- Logging
- Required patches

---

# Error Message Validation

Verify that documented and actual error messages do not reveal:

- Stack traces
- Credentials
- Internal paths
- Database schema
- Sensitive system details

---

# Release Notes

Release notes should accurately identify relevant:

- Security fixes
- Breaking changes
- Known security issues
- Upgrade instructions
- Security configuration changes

---

# Verification vs. Validation of Documentation

```text
Verification
→ Is documentation technically correct?

Validation
→ Does documentation meet user/operational needs?
```

---

# 6.4 Identify Undocumented Functionality

Undocumented functionality may represent:

- Forgotten development features
- Debug interfaces
- Hidden APIs
- Backdoors
- Test accounts
- Disabled-but-reachable functionality
- Easter eggs

---

# Why Undocumented Functionality Is Dangerous

It may:

- Bypass security reviews
- Lack authentication
- Lack testing
- Increase attack surface
- Introduce hidden access

---

# Example

Application documentation exposes:

```text
/api/v1/*
```

Testing discovers:

```text
/debug/admin
```

which is not documented.

Think:

> **Undocumented functionality / attack surface expansion**

---

# Debug Features

Development features should not automatically reach production.

Examples:

- Debug consoles
- Test credentials
- Verbose errors
- Developer APIs

---

# Exam Rule

Unexpected functionality should be:

> Investigated, authorized, documented, tested, and removed if unnecessary.

---

# 6.5 Analyze Security Implications of Test Results

Finding vulnerabilities is not enough.

Organizations must determine:

- Severity
- Exploitability
- Business impact
- Required remediation
- Release impact
- Retest requirements

---

# Test Finding Workflow

```text
Finding
   ↓
Validate
   ↓
Classify
   ↓
Assess Risk
   ↓
Prioritize
   ↓
Remediate
   ↓
Retest
   ↓
Close
```

---

# False Positive

A false positive occurs when a tool reports a vulnerability that does not actually exist.

Example:

Scanner reports SQL injection.

Manual analysis confirms:

> Parameterized queries prevent injection.

---

# False Negative

A false negative occurs when:

> A real vulnerability exists but testing fails to detect it.

False negatives can be more dangerous because they create:

> False confidence.

---

# True Positive

Finding exists and is correctly detected.

---

# True Negative

No vulnerability exists and testing correctly indicates none.

---

# Quick Table

| Reality | Test Says Vulnerable | Test Says Safe |
|---|---|---|
| Vulnerability exists | **True Positive** | **False Negative** |
| No vulnerability | **False Positive** | **True Negative** |

---

# Break/Build Criteria

Test results may determine whether software is allowed to continue through the lifecycle.

Example:

```text
Critical vulnerability found
       ↓
Build / Release blocked
       ↓
Fix
       ↓
Retest
```

---

# Exam Rule

Release decisions should follow:

> Defined security acceptance and risk criteria.

Not:

> Developer preference.

---

# Product Management Impact

Some findings may affect:

- Release schedule
- Feature scope
- Cost
- Risk acceptance
- Customer notification
- Roadmap

Security teams should communicate findings in:

> Business-relevant terms.

---

# Prioritization

Prioritize using:

```text
Technical Severity
+
Exploitability
+
Exposure
+
Business Impact
+
Threat Context
```

Do not prioritize based solely on:

> Number of vulnerabilities.

---

# Remediation Verification

After a developer claims an issue is fixed:

> Retest it.

Do not assume:

> Code change = verified remediation.

---

# 6.6 Classify and Track Security Errors

Security defects should be formally tracked.

Common states:

```text
New
 ↓
Validated
 ↓
Assigned
 ↓
In Progress
 ↓
Fixed
 ↓
Retest
 ↓
Closed
```

---

# Defect vs. Error vs. Vulnerability

Terminology varies, but a useful distinction:

## Error

Human mistake.

Example:

> Developer misunderstands authorization requirement.

## Defect / Bug

Software implementation problem caused by an error.

Example:

> Missing authorization check.

## Vulnerability

A weakness that may be exploited to violate security.

Example:

> Attacker can modify another user's account.

---

# Shortcut

```text
Error
→ Human mistake

Defect
→ Software flaw

Vulnerability
→ Security-exploitable weakness
```

---

# Bug Tracking

Security defects should include information such as:

- Description
- Location
- Severity
- Reproduction steps
- Owner
- Status
- Discovery date
- Remediation target
- Retest result

---

# Avoid Duplicate Closure Without Evidence

A defect should not be closed simply because:

> Developer says it is fixed.

Better:

> Verify remediation through retesting.

---

# CVSS

**Common Vulnerability Scoring System**

CVSS provides a standardized way to express:

> Technical severity of vulnerabilities.

---

# CVSS Is Not Business Risk

Important exam distinction:

```text
CVSS
→ Technical vulnerability severity.

Business Risk
→ Technical severity + context + impact.
```

Example:

Two applications both have CVSS 9.8 vulnerabilities.

Application A:

> Isolated lab environment.

Application B:

> Internet-facing payment platform.

Business risk is likely much higher for:

> Application B.

---

# Risk Scoring

Risk scoring may consider:

- CVSS
- Asset value
- Exposure
- Threat likelihood
- Business impact
- Existing controls

---

# 6.7 Secure Test Data

Test data can itself create security and privacy risk.

Bad practice:

> Copy the full production database into a developer laptop.

---

# Test Data Requirements

Test data should be:

- Representative
- Sufficient for testing
- Properly protected
- Privacy-aware
- Controlled throughout its lifecycle

---

# Generate Synthetic Test Data

Synthetic data is artificially created for testing.

Benefits:

- Avoids exposing real customer information
- Easier privacy management
- Can deliberately include edge cases

---

# Example

Instead of:

```text
Real customer:
Alice Smith
Real national ID
Real bank account
```

Use:

```text
Synthetic User 123
Synthetic identifier
Synthetic account number
```

---

# Production-Representative Data

Synthetic data should still resemble production where required.

Characteristics may include:

- Similar formats
- Similar distributions
- Similar volumes
- Similar relationships

---

# Referential Integrity

Test data should preserve relationships where those relationships matter.

Example:

```text
Customer ID 100
      ↓
Orders 5001, 5002
```

If data masking changes the customer identifier:

> Related order references should remain valid.

---

# Statistical Quality

Sometimes testing requires representative statistical characteristics.

Example:

Production:

```text
10% small transactions
70% normal transactions
20% large transactions
```

Test data may need similar distribution for realistic performance or fraud testing.

---

# Reusing Production Data

Sometimes organizations reuse production-derived data.

This creates risks involving:

- PII
- Financial information
- Credentials
- Health information
- Trade secrets

Therefore controls are required.

---

# Sanitization

Sanitization removes or modifies sensitive information.

Think:

> Clean sensitive content before testing.

---

# Anonymization

Anonymization attempts to remove the ability to identify individuals.

Think:

> Identity cannot reasonably be restored.

---

# Tokenization

Tokenization replaces sensitive values with surrogate tokens.

Example:

```text
4111 1111 1111 1111
        ↓
TKN-9348-A
```

---

# Obfuscation / Masking

Masking hides portions of values.

Example:

```text
john.smith@example.com
        ↓
j***@example.com
```

This may reduce exposure but is not necessarily:

> True anonymization.

---

# Data Aggregation Risk

Even anonymized individual data may become sensitive when combined with other datasets.

Example:

```text
ZIP code
+
Birth date
+
Gender
```

may allow re-identification.

This is:

> **Aggregation / inference risk**

---

# Test Data Exam Rule

Best practice is generally:

> Use synthetic data when practical.

If production data must be used:

> Apply appropriate protection, minimization, sanitization, anonymization, or tokenization.

---

# Test Data Lifecycle

Protect test data through:

```text
Generation
   ↓
Transfer
   ↓
Storage
   ↓
Use
   ↓
Backup
   ↓
Disposal
```

Test environments are not automatically allowed weaker data protection.

---

# Common Exam Trap

Wrong:

> It's only a test environment, so production privacy requirements do not apply.

Better:

> Sensitive information remains sensitive regardless of environment.

---

# 6.8 Perform Verification and Validation Testing

Verification and validation are fundamental CSSLP concepts.

---

# Verification

Verification asks:

> **Did we build the software correctly?**

It checks implementation against:

- Requirements
- Specifications
- Design

Examples:

- Code review
- Security testing
- Requirements traceability
- Unit testing

Memory phrase:

> **Built right?**

---

# Validation

Validation asks:

> **Did we build the right software?**

It determines whether software satisfies:

- Intended use
- Stakeholder needs
- Operational needs

Examples:

- Acceptance testing
- User evaluation
- Operational testing

Memory phrase:

> **Built the right thing?**

---

# Verification vs. Validation Shortcut

```text
Verification
→ Built RIGHT?

Validation
→ Built the RIGHT thing?
```

---

# Independent Verification and Validation — IV&V

Independent V&V is performed by a party sufficiently independent from the developers.

Benefits:

- Reduced bias
- Independent evidence
- Fresh perspective
- Increased assurance

Especially valuable for:

- High-risk systems
- Critical systems
- Regulated systems

---

# Internal V&V

Internal V&V may be performed by an internal team separate from the development team.

Independence can exist:

> Within the organization.

It does not necessarily require an external company.

---

# Acceptance Testing

Acceptance testing determines whether the product meets stakeholder acceptance criteria.

This is strongly associated with:

> **Validation**

---

# High-Value Exam Distinctions

## Functional vs. Nonfunctional Testing

```text
Functional
→ Does the control work?

Nonfunctional
→ How does it perform under constraints?
```

---

## Known vs. Unknown Environment

```text
Known
→ Tester has internal knowledge.

Unknown
→ Tester has little internal knowledge.
```

---

## SAST vs. DAST vs. IAST

```text
SAST
→ Static code analysis.

DAST
→ External testing of running application.

IAST
→ Runtime testing with internal instrumentation.
```

---

## Vulnerability Scan vs. Penetration Test

```text
Vulnerability Scan
→ Identify possible weaknesses.

Penetration Test
→ Attempt exploitation.
```

---

## Generation vs. Mutation Fuzzing

```text
Generation
→ Create inputs from model/spec.

Mutation
→ Modify existing inputs.
```

---

## Fault Injection vs. Stress Testing

```text
Fault Injection
→ Fail a component intentionally.

Stress Testing
→ Exceed operating limits.
```

---

## Unit vs. Integration Testing

```text
Unit
→ Test one small component.

Integration
→ Test components together.
```

---

## Regression vs. Retest

```text
Retest
→ Did this particular defect get fixed?

Regression
→ Did the change break anything else or reintroduce previous behavior?
```

---

## Code Coverage vs. Security Coverage

```text
Code Coverage
→ What code executed?

Security Coverage
→ Were relevant threats and security requirements tested?
```

High code coverage does not guarantee high security coverage.

---

## False Positive vs. False Negative

```text
False Positive
→ Reported vulnerability does not exist.

False Negative
→ Real vulnerability was missed.
```

---

## CVSS vs. Business Risk

```text
CVSS
→ Technical severity.

Business Risk
→ Severity + exposure + context + impact.
```

---

## Synthetic vs. Production Test Data

```text
Synthetic
→ Artificial but representative.

Production
→ Real operational data carrying real privacy/security risk.
```

---

## Verification vs. Validation

```text
Verification
→ Built right?

Validation
→ Built the right thing?
```

---

# Common CSSLP Domain 6 Exam Traps

## Trap 1 — Pen Test Equals Complete Testing

Wrong:

> The application passed penetration testing, so no other security testing is required.

Better:

> Penetration testing is one technique within a broader strategy.

---

## Trap 2 — DAST Replaces SAST

Wrong:

> DAST found no vulnerabilities, therefore source analysis is unnecessary.

Better:

> Different testing methods identify different classes of problems.

---

## Trap 3 — 100% Code Coverage Means Secure

Wrong:

> Every line executed, therefore all security issues were tested.

Better:

> Coverage does not prove quality or malicious-path testing.

---

## Trap 4 — Scanner Finding Equals Proven Vulnerability

Wrong:

> Scanner finding automatically proves exploitation.

Better:

> Validate findings and assess exploitability.

---

## Trap 5 — Ignore False Positives by Disabling Rules

Wrong:

> Scanner creates false positives, so disable security scanning.

Better:

> Tune rules and validate findings.

---

## Trap 6 — Test Only Expected Behavior

Wrong:

> All legitimate transactions succeed, therefore testing is complete.

Better:

> Security testing must include negative, misuse, and abuse scenarios.

---

## Trap 7 — Production Data Is Best Test Data

Wrong:

> Real production data always gives the most accurate tests.

Better:

> Use representative synthetic data when practical to reduce privacy/security risk.

---

## Trap 8 — Test Environment Can Have Weak Security

Wrong:

> Test systems are not public, so production data can be stored there unprotected.

Better:

> Data sensitivity does not change merely because the environment is non-production.

---

## Trap 9 — Fix Means Close

Wrong:

> Developer marked issue fixed, so close it.

Better:

> Retest before closure.

---

## Trap 10 — CVSS Determines Release Decision Alone

Wrong:

> Highest CVSS finding automatically blocks every release.

Better:

> Follow defined break/build criteria and risk context.

---

## Trap 11 — Bug Bounty Replaces Internal Testing

Wrong:

> External researchers will find our vulnerabilities.

Better:

> Bug bounty complements—not replaces—internal assurance activities.

---

## Trap 12 — Fuzzing Only Tests Random Data

Wrong:

> Fuzzing is simply random bytes.

Better:

> Fuzzing may be generation-based, mutation-based, structured, or protocol-aware.

---

## Trap 13 — Hidden Functionality Is Harmless If Unused

Wrong:

> Nobody knows about the debug endpoint, so it is safe.

Better:

> Undocumented functionality increases attack surface and should be investigated.

---

## Trap 14 — Test Results Are Purely Technical

Wrong:

> Security testing only concerns developers.

Better:

> Findings may affect product management, release decisions, legal obligations, and risk acceptance.

---

## Trap 15 — Independent Means External Vendor

Wrong:

> IV&V always requires another company.

Better:

> Independence can be organizational, provided sufficient separation exists.

---

# Scenario Recognition Examples

## Scenario 1

A security tester receives:

- Source code
- Architecture diagrams
- Administrative credentials

Think:

> **Known-environment testing**

---

## Scenario 2

A tester receives only:

> The application's public URL.

Think:

> **Unknown-environment testing**

---

## Scenario 3

A tool sends malicious HTTP requests to the running web application without inspecting source code.

Think:

> **DAST**

---

## Scenario 4

A runtime security tool instruments the application and observes how tainted user input flows into SQL operations.

Think:

> **IAST**

---

## Scenario 5

A tester discovers a possible SQL injection and attempts to extract database information.

Think:

> **Penetration testing**

---

## Scenario 6

A valid image file is modified thousands of ways and submitted to an image parser.

Think:

> **Mutation-based fuzzing**

---

## Scenario 7

A protocol specification is used to automatically generate malformed packets.

Think:

> **Generation-based fuzzing**

---

## Scenario 8

The security team disconnects the database during a transaction to observe how the application behaves.

Think:

> **Fault injection**

---

## Scenario 9

The team generates ten times the normal traffic until the application fails.

Think:

> **Stress testing**

---

## Scenario 10

A cryptographic reset token can be predicted because the application uses a weak random-number generator.

Think:

> **PRNG / entropy validation**

---

## Scenario 11

A developer fixes an authorization bypass.

The team reruns the original exploit test.

Think:

> **Retesting**

The team also reruns the broader security test suite.

Think:

> **Regression testing**

---

## Scenario 12

Two services work correctly independently but fail authentication when connected together.

Think:

> **Integration testing**

---

## Scenario 13

Testing discovers:

```text
/admin-debug
```

but no documentation mentions this endpoint.

Think:

> **Undocumented functionality**

---

## Scenario 14

A scanner assigns a vulnerability:

```text
CVSS 9.8
```

Think:

> **Technical severity**

Do not automatically assume:

> Business risk = 9.8.

---

## Scenario 15

Developers copy the customer production database to QA for testing.

Think:

> **Test-data security/privacy risk**

---

## Scenario 16

Real names are replaced with irreversible synthetic identities while maintaining realistic transaction distributions.

Think:

> **Anonymized / representative test data**

---

## Scenario 17

An independent security group checks whether the implementation satisfies documented security requirements.

Think:

> **Independent Verification**

---

## Scenario 18

Business users determine whether the application meets intended operational requirements.

Think:

> **Validation / acceptance testing**

---

# Security Test Strategy Workflow

```text
Security Requirements
        ↓
Risk Assessment
        ↓
Threat Model
        ↓
Define Test Scope
        ↓
Select Techniques
        ↓
Design Test Cases
        ↓
Prepare Environment & Data
        ↓
Execute
        ↓
Analyze Findings
        ↓
Remediate
        ↓
Retest
        ↓
Regression Test
        ↓
Release Decision
```

---

# Security Test Case Checklist

```text
[ ] Security requirement tested?

[ ] Positive case tested?

[ ] Negative case tested?

[ ] Authorization bypass attempted?

[ ] Authentication failure tested?

[ ] Invalid input tested?

[ ] Boundary values tested?

[ ] Misuse case tested?

[ ] Abuse case tested?

[ ] Failure state tested?

[ ] Resource exhaustion considered?

[ ] Trust boundaries tested?

[ ] External interfaces tested?

[ ] Error handling tested?

[ ] Logging behavior tested?

[ ] Data confidentiality tested?

[ ] Data integrity tested?

[ ] Test results reproducible?

[ ] Expected result documented?
```

---

# Secure Test Data Checklist

```text
[ ] Is production data actually necessary?

[ ] Can synthetic data be used?

[ ] Is sensitive information removed?

[ ] Is anonymization sufficient?

[ ] Are token mappings protected?

[ ] Is referential integrity maintained?

[ ] Is statistical quality representative?

[ ] Is test data encrypted where required?

[ ] Is access controlled?

[ ] Is data retained only as long as necessary?

[ ] Is secure destruction planned?
```

---

# Fast Memory Sheet

```text
TEST STRATEGY
→ What, how, when, and who will test.

FUNCTIONAL TESTING
→ Does the security control work?

NONFUNCTIONAL TESTING
→ Reliability, performance, scalability.

KNOWN ENVIRONMENT
→ Tester knows system internals.

UNKNOWN ENVIRONMENT
→ Tester has little internal knowledge.

ACCEPTANCE TESTING
→ Does it meet acceptance criteria?

DAST
→ Test running application externally.

IAST
→ Runtime testing with internal visibility.

PENETRATION TEST
→ Attempt actual exploitation.

FUZZING
→ Send unexpected or malformed input.

GENERATION FUZZING
→ Create input from specification/model.

MUTATION FUZZING
→ Alter existing input.

FAULT INJECTION
→ Deliberately cause component failure.

STRESS TEST
→ Exceed expected capacity.

ENTROPY TESTING
→ Validate unpredictability.

UNIT TEST
→ Test one component.

CODE COVERAGE
→ Measure executed code.

REGRESSION TEST
→ Make sure changes didn't break existing behavior.

INTEGRATION TEST
→ Test components together.

CONTINUOUS TESTING
→ Test throughout CI/CD.

MISUSE TEST
→ Test malicious behavior scenarios.

UNDOCUMENTED FUNCTIONALITY
→ Hidden/unexpected features or interfaces.

FALSE POSITIVE
→ Reported issue does not exist.

FALSE NEGATIVE
→ Real issue was missed.

CVSS
→ Technical vulnerability severity.

SYNTHETIC DATA
→ Artificial representative test data.

VERIFICATION
→ Built right?

VALIDATION
→ Built the right thing?
```

---

# Hardest Domain 6 Distinctions to Memorize

| Question Is Asking... | Likely Concept |
|---|---|
| Test running app from outside? | **DAST** |
| Runtime analysis with internal instrumentation? | **IAST** |
| Analyze source without execution? | **SAST** |
| Try to exploit identified weaknesses? | **Penetration Test** |
| Generate malformed inputs from specification? | **Generation Fuzzing** |
| Modify valid samples? | **Mutation Fuzzing** |
| Deliberately disable a dependency? | **Fault Injection** |
| Push beyond operating capacity? | **Stress Testing** |
| Did the specific fix work? | **Retest** |
| Did the fix break anything else? | **Regression Test** |
| Test one function independently? | **Unit Test** |
| Test services communicating together? | **Integration Test** |
| Measure which code executed? | **Code Coverage** |
| Scanner says vulnerability but none exists? | **False Positive** |
| Vulnerability exists but scanner missed it? | **False Negative** |
| Technical vulnerability severity? | **CVSS** |
| Artificial realistic customer records? | **Synthetic Data** |
| Built according to specification? | **Verification** |
| Meets intended business need? | **Validation** |

---

# CSSLP Domain 6 Exam Strategy

Domain 6 questions frequently give you several valid testing methods.

The key is to identify:

> **What exactly is the testing objective?**

---

# Example 1

Question:

> The organization wants to identify vulnerabilities in the application while it is running, without access to source code.

Think:

```text
Running application
+
External perspective
=
DAST
```

---

# Example 2

Question:

> The organization wants to identify where untrusted input travels through the running application's source-level execution.

Think:

```text
Runtime
+
Internal instrumentation
=
IAST
```

---

# Example 3

Question:

> A tester wants to determine whether a suspected vulnerability can actually be exploited.

Think:

> **Penetration testing**

---

# Example 4

Question:

> After correcting a vulnerability, what should the team do FIRST?

Generally:

> **Retest the original vulnerability.**

Then:

> Perform appropriate regression testing.

---

# Example 5

Question:

> What is the BEST test data for a QA environment containing sensitive customer workflows?

Usually prefer:

> **Representative synthetic data**

over copying uncontrolled production data.

---

# Prevention vs. Detection vs. Validation

Know what the test is trying to accomplish.

```text
Testing
→ Discover defects and validate behavior.

Control
→ Prevent/detect security events during operation.

Remediation
→ Correct identified defect.
```

Do not confuse a security test with a production security control.

---

# Test Early and Continuously

A key CSSLP principle is:

```text
Requirements
     ↓
Design
     ↓
Implementation
     ↓
Testing
```

Security testing should not begin only after implementation is complete.

Instead:

> Security test design should begin when security requirements are defined.

---

# Shift Left

"Shift left" generally means performing security activities earlier in development.

Examples:

- Security requirements
- Threat modeling
- SAST
- Unit security tests
- Dependency analysis

Benefits:

> Earlier defects are usually easier and cheaper to correct.

---

# But Do Not Only Shift Left

Runtime and production-like testing still matters.

A strong testing program combines:

```text
Early Testing
     +
Runtime Testing
     +
Integration Testing
     +
Pre-Release Testing
```

---

# Layered Testing Strategy

A mature approach may include:

```text
Unit Security Tests
       ↓
SAST
       ↓
SCA
       ↓
Integration Tests
       ↓
IAST
       ↓
DAST
       ↓
Penetration Test
       ↓
Acceptance Testing
```

No single technique provides complete coverage.

---

# Domain 6 Master Sequence

Memorize:

```text
Plan
 ↓
Design Tests
 ↓
Prepare Environment
 ↓
Secure Test Data
 ↓
Execute
 ↓
Find
 ↓
Validate
 ↓
Score
 ↓
Prioritize
 ↓
Fix
 ↓
Retest
 ↓
Regression Test
 ↓
Accept / Release
```

---

# Final Exam Rules

### Rule 1

> Security testing should derive from requirements, architecture, threats, and risk.

### Rule 2

> Functional testing asks whether a security feature works; nonfunctional testing examines qualities such as reliability, performance, and scalability.

### Rule 3

> Known-environment testing gives the tester internal knowledge; unknown-environment testing more closely resembles an external attacker perspective.

### Rule 4

> DAST tests the running application externally; IAST adds internal runtime visibility.

### Rule 5

> Vulnerability scanning identifies possible weaknesses; penetration testing attempts exploitation.

### Rule 6

> Fuzzing tests unexpected inputs; generation-based fuzzing creates inputs, while mutation-based fuzzing modifies existing samples.

### Rule 7

> Fault injection deliberately creates failures; stress testing pushes the system beyond normal capacity.

### Rule 8

> Cryptographic security depends on sufficient unpredictability and entropy where randomness is required.

### Rule 9

> High code coverage does not automatically mean strong security coverage.

### Rule 10

> Retesting verifies a specific remediation; regression testing checks that changes did not break existing behavior.

### Rule 11

> Integration testing is critical because secure components can become insecure when their assumptions conflict.

### Rule 12

> Misuse and abuse cases should become executable security test cases.

### Rule 13

> Undocumented functionality should be investigated because it expands attack surface.

### Rule 14

> Findings should be validated, risk-ranked, tracked, remediated, and retested.

### Rule 15

> CVSS measures technical severity, not complete business risk.

### Rule 16

> Test environments do not make sensitive data less sensitive.

### Rule 17

> Prefer representative synthetic test data when practical.

### Rule 18

> Verification asks whether the system was built correctly; validation asks whether the correct system was built.

### Rule 19

> Independent verification and validation provides stronger assurance by reducing developer bias.

### Rule 20

> If two testing answers appear correct, select the technique that most directly satisfies the stated testing objective.

---

## Disclaimer

This is an independent CSSLP study guide and is not an official ISC2 publication or a collection of official exam questions.
