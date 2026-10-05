# CSSLP Domain 5 Study Guide
## Secure Software Implementation

This study guide focuses on **CSSLP Domain 5: Secure Software Implementation**.

Domain 5 is about turning secure requirements and architecture into **secure code, components, configurations, and build artifacts**.

The major areas are:

1. Secure coding practices
2. Code analysis for security risks
3. Security controls
4. Risk treatment
5. Component evaluation and integration
6. Security during the build process

---

# Domain 5 Mental Model

Think of Domain 5 as:

```text
Requirements
     ↓
Architecture
     ↓
Secure Coding
     ↓
Code Analysis
     ↓
Component Analysis
     ↓
Security Controls
     ↓
Risk Treatment
     ↓
Secure Build
     ↓
Trusted Artifact
```

Domain 4 asks:

> How should the system be securely designed?

Domain 5 asks:

> How do we securely implement that design?

---

# Fast Exam Recognition Table

| If You See... | Think... |
|---|---|
| Security rules expressed as configuration or policy | **Declarative Security** |
| Security explicitly enforced in application code | **Imperative Security** |
| Two threads modify shared data simultaneously | **Concurrency / Race Condition** |
| Validate data before using it | **Input Validation** |
| Remove dangerous content from input | **Sanitization** |
| Prevent browser interpretation of data as code | **Output Encoding** |
| Stack trace shown to users | **Improper Error Handling** |
| Password/token written to logs | **Insecure Logging** |
| Session ID predictable or reusable | **Session Management** |
| External API/library receives trusted treatment | **Trust Boundary / Dependency Risk** |
| Application consumes unlimited CPU/memory | **Resource Management** |
| Default credentials remain enabled | **Secure Configuration Management** |
| Sensitive value replaced by meaningless token | **Tokenization** |
| Untrusted code executed separately | **Sandboxing / Isolation** |
| Algorithm or key selection | **Cryptography** |
| User gets only required functions | **Access Control / Least Privilege** |
| Review source without running it | **SAST** |
| Human reviews code | **Manual Code Review** |
| Common weakness category | **CWE** |
| Common web application risks | **OWASP Top 10** |
| Reusing vulnerable library | **Software Composition Analysis** |
| Hidden privileged functionality | **Backdoor** |
| Malicious function activates on condition | **Logic Bomb** |
| Suspicious encrypted/packed code | **High Entropy** |
| Detect unauthorized file changes | **File Integrity Monitoring** |
| System-of-systems integration | **Trust Contracts / Integration Security** |
| Protect built binary from modification | **Code Signing / Anti-Tampering** |
| Compiler security settings disabled | **Compiler Switches** |
| Compiler warning ignored | **Address Compiler Warnings** |

---

# 5.1 Adhere to Relevant Secure Coding Practices

Secure coding means implementing software according to:

- Security requirements
- Secure coding standards
- Organizational policy
- Industry guidance
- Regulatory requirements
- Secure design principles

The objective is not merely:

> Code that works.

The objective is:

> Code that works securely under both expected and hostile conditions.

---

# Secure Coding Standards

Secure coding standards establish consistent implementation practices.

They may define requirements for:

- Input validation
- Error handling
- Memory management
- Authentication
- Authorization
- Cryptography
- Logging
- Sessions
- APIs
- Dependencies
- Secrets
- Concurrency

### Exam Rule

When developers repeatedly make the same class of security mistake:

> Improve the coding standard, training, tooling, and feedback loop rather than only fixing individual defects.

---

# Declarative vs. Imperative Security

This distinction is important.

## Declarative Security

Security policy is expressed through:

- Configuration
- Attributes
- Metadata
- Framework policy
- Access rules

rather than custom application logic.

Example:

```text
ROLE_ADMIN → /admin/*
ROLE_USER  → /account/*
```

The framework enforces the policy.

Think:

> **Describe what security policy should apply.**

---

## Imperative Security

Security logic is implemented explicitly in code.

Example:

```text
if current_user.role == "admin":
    allow()
else:
    deny()
```

Think:

> **Program the security decision directly.**

---

# Declarative vs. Imperative Shortcut

```text
Declarative
→ Define the policy.

Imperative
→ Write code that enforces the policy.
```

---

# Exam Consideration

Declarative controls may provide:

- Centralization
- Consistency
- Easier review

Imperative controls may provide:

- Flexibility
- Application-specific logic

Neither is always superior.

The best choice depends on:

> The security requirement and implementation context.

---

# Concurrency

Concurrency occurs when multiple:

- Threads
- Processes
- Transactions
- Requests

operate at the same time.

Security problems can occur when operations unexpectedly interact.

---

# Race Condition

A race condition exists when the outcome depends on timing or execution order.

Example:

```text
Thread A:
Check account balance = €100

Thread B:
Check account balance = €100

Thread A:
Withdraw €80

Thread B:
Withdraw €80
```

Both operations may succeed even though the account does not contain €160.

---

# TOCTOU

**Time of Check to Time of Use**

A specific race-condition pattern.

```text
Check permission
      ↓
Something changes
      ↓
Use resource
```

Example:

```text
Application checks:
"This file is safe."

Attacker replaces file.

Application opens attacker-controlled file.
```

Think:

> **The state changed between checking and using it.**

---

# Thread Safety

Thread-safe code behaves correctly when multiple threads access it concurrently.

Possible controls include:

- Locks
- Atomic operations
- Synchronization
- Transaction isolation
- Immutable data structures

### Exam Rule

Do not simply add more threads to solve a concurrency security problem.

The implementation must correctly coordinate access to shared resources.

---

# Database Concurrency Controls

Databases can protect consistency using:

- Transactions
- Locks
- Isolation levels
- Atomic operations

A secure implementation should prevent concurrent operations from bypassing:

- Balance limits
- Inventory restrictions
- Authorization state
- Business rules

---

# Input Validation

Input validation verifies that data meets expected requirements **before it is processed**.

Validate:

- Type
- Length
- Range
- Format
- Character set
- Structure
- Business rules

---

# Allowlisting vs. Denylisting

## Allowlisting

Define what is permitted.

Example:

```text
Product quantity:
Allowed = integers 1 through 100
```

Everything else is rejected.

---

## Denylisting

Define known forbidden inputs.

Example:

```text
Reject:
<script>
SELECT
DROP
```

Problem:

Attackers may find variants that are not on the list.

---

# Exam Rule

For validation:

> **Allowlisting is generally preferred when practical.**

Define what valid input looks like rather than attempting to enumerate every malicious input.

---

# Validate on the Server

Client-side validation improves usability but is not a security boundary.

Bad assumption:

```text
Browser prevents quantity > 10
therefore
Server does not validate quantity
```

Attackers can bypass browser controls.

Correct approach:

> Validate untrusted input at the trusted processing boundary.

---

# Canonicalization

Different representations may refer to the same logical value.

Example:

```text
../
%2e%2e%2f
encoded variants
```

Security controls should evaluate data in a consistent canonical form.

### Exam Trap

Validating one representation and decoding it later can allow validation bypasses.

---

# Validation vs. Sanitization

## Validation

Answers:

> Is this input acceptable?

Possible result:

```text
Accept
or
Reject
```

---

## Sanitization

Attempts to transform or remove dangerous content.

Example:

```text
Input:
<script>alert(1)</script>

Sanitized:
alert(1)
```

---

# Exam Preference

When strict validation is possible:

> Reject invalid input rather than attempting to repair arbitrary malicious input.

Sanitization is still useful where transformation is required.

---

# Input Validation vs. Output Encoding

This distinction is extremely important.

## Input Validation

Protects the application from unexpected input.

Think:

> Is this input valid?

---

## Output Encoding

Protects the destination interpreter from treating data as executable syntax.

Think:

> How should this value be represented safely in this output context?

---

# Example: XSS

User enters:

```text
<script>alert(1)</script>
```

When displayed in HTML, encode it so the browser treats it as:

> Text

rather than:

> JavaScript

---

# Memory Shortcut

```text
Input Validation
→ Is the input allowed?

Output Encoding
→ Ensure data remains data.
```

---

# Context Matters

Output encoding depends on destination context.

Examples include:

- HTML
- HTML attribute
- JavaScript
- URL
- SQL
- XML

One encoding method should not automatically be reused for every context.

---

# Parameterized Queries

For database queries, do not build commands by concatenating untrusted strings.

Unsafe concept:

```text
"SELECT * FROM users WHERE name = '" + input + "'"
```

Better:

> Use parameterized queries / prepared statements.

This separates:

```text
Code
from
Data
```

and helps prevent SQL injection.

---

# Error and Exception Handling

Error handling should:

- Fail safely
- Avoid exposing sensitive details
- Log useful diagnostics
- Preserve system integrity
- Provide appropriate user feedback

---

# Dangerous Error Information

Do not expose information such as:

- Stack traces
- Database connection strings
- File paths
- SQL statements
- Internal hostnames
- Secrets
- Cryptographic keys

Bad:

```text
Login failed because:
SQL connection db-prod-17.company.internal failed
using account admin-db.
```

Better:

```text
Unable to complete request.
```

Detailed information can be recorded securely for administrators.

---

# Fail Secure

If an exception occurs during authorization:

Bad:

```text
Authorization error
     ↓
Allow request
```

Better:

```text
Authorization error
     ↓
Deny request safely
```

---

# Exception Handling Trap

Do not use:

```text
catch all exceptions
ignore error
continue execution
```

for security-sensitive operations.

Unexpected failures should not silently bypass security controls.

---

# Output Sanitization and Encoding

Output handling protects:

- Users
- Browsers
- Downstream applications
- Log systems
- APIs

Examples include:

- HTML encoding
- URL encoding
- Escaping
- Safe serialization
- Data obfuscation where appropriate

---

# Obfuscation

Obfuscation makes information harder to interpret.

Example:

```text
1234-5678-9012-3456

Displayed:
****-****-****-3456
```

However:

> Obfuscation is not equivalent to cryptographic encryption.

---

# Secure Logging and Auditing

Logs support:

- Accountability
- Incident response
- Monitoring
- Troubleshooting
- Forensics

But logging can itself create security risk.

---

# What Should Be Logged?

Examples:

- Authentication events
- Authorization failures
- Privileged actions
- Security configuration changes
- Critical transactions
- Security-control failures

---

# What Should Usually NOT Be Logged?

Avoid recording unnecessary:

- Passwords
- Full authentication tokens
- Private cryptographic keys
- Sensitive personal data
- Payment card details
- Secret answers

---

# Log Confidentiality

Logs may contain sensitive operational data.

Therefore protect them with:

- Access controls
- Encryption where appropriate
- Retention controls

---

# Log Integrity

Attackers may attempt to erase evidence.

Protect logs using:

- Restricted write access
- Centralized logging
- Integrity protection
- Append-only mechanisms
- Remote collection

---

# Log Injection

Untrusted input may contain:

```text
newline characters
fake log entries
control sequences
```

Applications should safely encode or structure log data.

---

# Session Management

A session maintains user state between requests.

Security requirements include:

- Unpredictable identifiers
- Secure transmission
- Expiration
- Rotation
- Revocation
- Logout
- Appropriate cookie protections

---

# Session Fixation

An attacker forces or predicts a victim's session identifier before login.

After authentication, the attacker reuses that identifier.

Mitigation:

> Generate or rotate the session identifier after authentication or privilege changes.

---

# Session Hijacking

An attacker steals a valid session identifier.

Possible protections:

- TLS
- Secure cookie settings
- Shorter session lifetime
- Rotation
- Reauthentication for sensitive operations

---

# Session Timeout

Consider:

## Idle Timeout

Ends session after inactivity.

## Absolute Timeout

Ends session after a maximum total duration.

---

# Privilege Change

When a user's privilege changes:

> Consider rotating/re-establishing session security context.

This helps prevent stale authorization state.

---

# Trusted and Untrusted APIs

APIs and libraries cross trust boundaries.

Never assume a component is trustworthy solely because it is:

- Internal
- Popular
- Open source
- Commercial
- Previously approved

---

# API Input

Validate:

- Parameters
- Authentication
- Authorization
- Size
- Format
- Rate
- State

---

# API Output

Do not blindly trust data returned by another service.

Example:

```text
External API
    ↓
Returns HTML
    ↓
Application displays it directly
```

This can create downstream injection problems.

---

# Third-Party Libraries

Before integrating libraries, consider:

- Known vulnerabilities
- Maintenance status
- Provenance
- Licensing
- Transitive dependencies
- Update process
- Required privileges

---

# Resource Management

Software must safely manage:

- CPU
- Memory
- Storage
- Network connections
- File handles
- Database connections
- Threads

Improper management can cause:

> Denial of service.

---

# Resource Exhaustion

Example:

An attacker uploads thousands of 5 GB files.

Without limits:

```text
Disk fills
     ↓
Application fails
```

Controls may include:

- Quotas
- Size limits
- Timeouts
- Rate limits
- Connection limits
- Resource cleanup

---

# Memory Management

Memory security issues may include:

- Buffer overflow
- Use-after-free
- Double free
- Memory leak
- Uninitialized memory

Memory-safe languages can reduce some classes of vulnerabilities, but secure logic is still required.

---

# Secure Configuration Management

Applications should have secure baseline configurations.

Consider:

- Default accounts
- Default passwords
- Debug mode
- Development endpoints
- TLS configuration
- Error verbosity
- File permissions
- Network exposure

---

# Secure Defaults

A secure application should ideally start in a secure state.

Bad:

```text
Authentication disabled by default
```

Better:

```text
Authentication required by default
```

Think:

> **Secure by default**

---

# Credential Management

Do not:

- Hard-code credentials
- Commit secrets to source control
- Store passwords in plaintext
- Share credentials unnecessarily

Use appropriate:

- Secret-management systems
- Key stores
- Environment-specific credential mechanisms

---

# Tokenization

Tokenization replaces sensitive information with a non-sensitive surrogate.

Example:

```text
Credit Card Number
4111111111111111
       ↓
Token
TKN-8F91A2
```

The mapping is maintained securely elsewhere.

---

# Tokenization vs. Encryption

## Encryption

```text
Plaintext
   ↓ Key
Ciphertext
   ↓ Key
Plaintext
```

Cryptographically reversible.

---

## Tokenization

```text
Sensitive Value
     ↓
Token Mapping System
     ↓
Token
```

The token may have no mathematical relationship to the original data.

---

# Memory Shortcut

```text
Encryption
→ Transform data cryptographically.

Tokenization
→ Replace data with a surrogate.
```

---

# Isolation

Isolation limits interaction between components.

Examples:

- Sandboxing
- Virtualization
- Containers
- Separation kernels

---

# Sandboxing

A sandbox limits what untrusted code can access.

Example:

```text
Untrusted Code
      ↓
Sandbox
      ↓
Restricted filesystem
Restricted network
Restricted privileges
```

---

# Isolation Principle

Ask:

> If this component is compromised, what else can it reach?

Good isolation reduces:

> **Blast radius**

---

# Containerization

Containers can provide application isolation but commonly share the host kernel.

Therefore:

> Container isolation should not automatically be treated as equivalent to separate physical systems.

---

# Separation Kernel

A separation kernel creates strongly isolated execution domains.

Its purpose is to:

> Prevent information and control flows between partitions except through explicitly allowed channels.

Think:

> **Strong partitioning**

---

# Cryptography

Implementation decisions may involve cryptography for:

- Payloads
- Individual fields
- Network transport
- Storage
- Authentication
- Digital signatures

---

# Cryptographic Algorithm Selection

Do not invent proprietary cryptography.

Prefer:

- Approved algorithms
- Approved libraries
- Appropriate key sizes
- Correct operating modes
- Managed keys

---

# Cryptographic Agility

Crypto agility means the system can replace:

- Algorithms
- Keys
- Protocol versions

without redesigning the entire application.

Example:

```text
Algorithm A deprecated
       ↓
Configuration change / controlled update
       ↓
Algorithm B
```

---

# Why Crypto Agility Matters

Cryptographic standards evolve.

Algorithms may become:

- Weak
- Deprecated
- Non-compliant

Hard-coding one algorithm throughout application logic makes migration difficult.

---

# Field-Level vs. Storage Encryption

## Storage Encryption

Protects storage media broadly.

Example:

> Full-disk encryption.

## Field-Level Encryption

Protects specific sensitive fields.

Example:

> Encrypt national identifiers inside a database.

### Exam Point

Disk encryption may not protect data from an application or user already authorized to read the mounted filesystem.

---

# Transport Encryption

Protects data while moving across networks.

Example:

> TLS

Think:

> **Data in transit**

---

# Access Control

Implementation must correctly enforce authorization.

Examples include:

- Function permissions
- Trust zones
- RBAC
- DAC
- MAC

---

# RBAC

**Role-Based Access Control**

Permissions are assigned to roles.

```text
User
 ↓
Role
 ↓
Permissions
```

Example:

```text
Alice → Finance Approver → Approve invoices
```

---

# DAC

**Discretionary Access Control**

The owner of an object can typically determine who receives access.

Think:

> **Owner-controlled permissions**

---

# MAC

**Mandatory Access Control**

Access is determined by centrally enforced labels or classifications.

Think:

```text
Subject Clearance
       +
Object Classification
       ↓
Access Decision
```

Users cannot simply override the policy.

---

# RBAC vs. DAC vs. MAC

```text
RBAC
→ Access based on role.

DAC
→ Owner decides.

MAC
→ Centrally enforced labels/classifications.
```

---

# Function-Level Authorization

A common application flaw occurs when:

> The UI hides a function but the server does not restrict it.

Example:

```text
Normal user cannot see:
/admin/deleteUser

but directly requesting the endpoint works.
```

This is an authorization failure.

---

# Processor Microarchitecture Security Extensions

Modern processors provide security features that software may leverage.

Examples can include:

- Memory protection
- Execution protection
- Hardware-backed isolation
- Trusted execution capabilities

The implementation should:

> Use available platform security mechanisms appropriately rather than bypassing them.

---

# 5.2 Analyze Code for Security Risks

Code should be examined before deployment.

Approaches include:

- SAST
- Manual code review
- Vulnerability knowledge bases
- Dependency analysis
- Malicious-code inspection

---

# Secure Code Reuse

Reuse can reduce risk when using:

- Vetted libraries
- Approved frameworks
- Well-maintained components

But reuse creates dependency risk.

Ask:

```text
Is it trusted?
Is it maintained?
Is it vulnerable?
Is it authentic?
Do we know its dependencies?
```

---

# OWASP Top 10

The OWASP Top 10 identifies major categories of web application security risk.

For CSSLP, understand its purpose:

> **Application security awareness and common web-risk categories**

Do not confuse it with:

> A complete secure coding standard or exhaustive vulnerability list.

---

# CWE

**Common Weakness Enumeration**

CWE catalogs classes of software weaknesses.

Examples conceptually include:

- Improper input validation
- Buffer errors
- Authorization errors
- Race conditions

Think:

> **Weakness category**

---

# CVE vs. CWE

Useful distinction:

```text
CWE
→ Type/class of weakness.

CVE
→ Specific publicly identified vulnerability.
```

Example:

```text
SQL injection class
→ CWE

Specific SQL injection flaw in Product X version Y
→ CVE
```

---

# SANS / CWE Top Software Errors

Lists such as the most dangerous software weaknesses help organizations focus secure coding and review efforts on high-risk defect categories.

---

# Static Application Security Testing — SAST

SAST analyzes application artifacts without executing the running application.

Often called:

> **White-box or static code analysis**

It can inspect:

- Source code
- Bytecode
- Binaries

---

# SAST Can Find

Examples:

- Dangerous function use
- Injection paths
- Hard-coded credentials
- Insecure API calls
- Certain memory issues
- Weak error handling

---

# SAST Advantages

- Runs early
- Can integrate into builds
- Gives source-level context
- Supports developer feedback

---

# SAST Limitations

May produce:

- False positives
- False negatives
- Limited runtime context

Therefore:

> Automated analysis should not be the only security assurance mechanism.

---

# Linting

A linter analyzes code for:

- Coding errors
- Style violations
- Suspicious constructs
- Unsafe patterns

Security-focused linting can identify known insecure coding practices.

---

# Code Coverage in Static Analysis

Code analysis tools may help identify:

- Portions reviewed
- Unanalyzed modules
- Unsupported languages/components

But:

> 100% tool coverage does not guarantee 100% security.

---

# Manual Code Review

Human review can identify:

- Business logic errors
- Authorization mistakes
- Dangerous assumptions
- Design violations
- Malicious logic

Humans can understand context that automated tools may miss.

---

# Peer Review

Peer review uses another developer or security reviewer to inspect changes.

Benefits:

- Second perspective
- Knowledge sharing
- Defect identification
- Reduced individual bias

---

# SAST vs. Manual Review

```text
SAST
→ Automated scale and pattern detection.

Manual Review
→ Human context and reasoning.
```

Best assurance often uses both.

---

# Malicious Code Inspection

Code analysis should also look for intentionally harmful functionality.

Examples:

- Backdoors
- Logic bombs
- Hidden accounts
- Credential stealing
- Unauthorized network calls

---

# Backdoor

A backdoor provides hidden or unauthorized access.

Example:

```text
if username == "support"
and password == "secret123":
    grant_admin()
```

Think:

> **Hidden bypass**

---

# Logic Bomb

A logic bomb activates when a condition is met.

Examples:

```text
Date == December 31
```

or:

```text
Employee account removed
```

Then malicious action executes.

Think:

> **Malicious code waiting for a trigger.**

---

# High Entropy

High-entropy regions of code or binaries can indicate:

- Encryption
- Compression
- Packing
- Obfuscation

These are not automatically malicious.

However, unexpected high entropy may warrant:

> Further investigation.

---

# Exam Trap

Do not conclude:

> High entropy = malware.

It is an indicator, not definitive proof.

---

# 5.3 Implement Security Controls

Security controls may be implemented directly into the application or its supporting environment.

Examples in Domain 5 include:

- Watchdogs
- File integrity monitoring
- Anti-malware

---

# Watchdog

A watchdog monitors whether a system or process is functioning correctly.

Example:

```text
Application
    ↓ heartbeat
Watchdog
```

If the heartbeat stops:

```text
Restart
Alert
Failover
```

Watchdogs can support:

- Availability
- Resilience
- Failure detection

---

# File Integrity Monitoring — FIM

FIM detects unexpected changes to important files.

Typical process:

```text
Known Good Hash
       ↓
Compare
       ↓
Current File Hash
       ↓
Changed?
```

Useful for:

- Executables
- Configuration files
- Critical system files

---

# FIM Primary Security Property

Think:

> **Integrity**

FIM detects change.

It does not necessarily:

> Prevent the original modification.

---

# Anti-Malware

Anti-malware controls attempt to:

- Detect malicious software
- Block execution
- Quarantine files
- Remove malware

It is one layer of:

> **Defense in depth**

---

# 5.4 Address Identified Security Risks

Not every identified issue is handled the same way.

Common risk strategies include:

```text
Avoid
Mitigate
Transfer
Accept
```

---

# Avoid

Eliminate the activity creating the risk.

Example:

> Remove an unnecessary vulnerable feature.

---

# Mitigate

Reduce likelihood or impact.

Example:

> Add authorization and input validation.

---

# Transfer

Shift some financial or operational consequences.

Examples:

- Insurance
- Contractual arrangements

Transfer does not necessarily eliminate accountability.

---

# Accept

Formally accept remaining risk.

### Critical Exam Rule

Developers should not independently accept significant business risk unless explicitly authorized.

Risk acceptance belongs to:

> The appropriate risk owner or authorized decision-maker.

---

# Fix vs. Compensating Control

The best approach is often to correct the root vulnerability.

If immediate remediation is impossible, a compensating control may temporarily reduce risk.

Example:

```text
Vulnerable endpoint
      +
Temporary network restriction
```

But:

> A compensating control should not automatically be treated as permanent remediation.

---

# Risk-Based Prioritization

Prioritize fixes based on:

- Exploitability
- Exposure
- Impact
- Business criticality
- Existing controls
- Threat likelihood

Do not rely solely on:

> Number of findings.

---

# 5.5 Evaluate and Integrate Components

Modern applications rarely consist entirely of custom code.

They integrate:

- Open-source libraries
- Commercial components
- APIs
- Services
- Frameworks
- Operating systems
- Containers

Each component introduces:

> Dependency and trust risk.

---

# Systems-of-Systems Integration

A system-of-systems connects independently managed systems.

Example:

```text
Customer Portal
      ↓
Identity Provider
      ↓
Payment Service
      ↓
Banking Network
```

Security must account for:

- Trust
- Authentication
- Data sharing
- Failure behavior
- Contractual obligations

---

# Trust Contracts

A trust contract defines security expectations between components or systems.

Questions include:

```text
Who authenticates whom?
What assertions are trusted?
What data can be exchanged?
What security controls are required?
What happens when trust fails?
```

---

# Integration Security Testing

A component may be secure independently but insecure when integrated.

Example:

```text
Service A assumes authenticated input.

Service B sends unauthenticated requests.
```

Therefore integration testing should verify:

> Security assumptions between components.

---

# Third-Party Code

Before reuse, evaluate:

- Origin
- Version
- Known vulnerabilities
- Maintenance activity
- Licensing
- Integrity
- Dependencies

---

# Open-Source Security

Open source is not automatically:

- Secure
- Insecure

Security depends on factors such as:

- Review
- Maintenance
- Community health
- Vulnerability history
- Update capability
- Provenance

---

# Software Composition Analysis — SCA

SCA analyzes third-party and open-source components.

It may identify:

- Components
- Versions
- Known vulnerabilities
- Licenses
- Transitive dependencies

---

# SAST vs. SCA

```text
SAST
→ Analyze your code for weaknesses.

SCA
→ Analyze third-party components/dependencies.
```

This is a high-value exam distinction.

---

# Transitive Dependencies

Your application may depend on:

```text
Library A
   ↓
Library B
   ↓
Library C
```

Even if you selected Library A:

> Vulnerability in Library C can still affect you.

SCA helps identify this dependency chain.

---

# Dependency Pinning

Pinning defines which component version is used.

This can improve:

- Repeatability
- Build consistency

But it can create risk if:

> An old vulnerable version remains pinned indefinitely.

Therefore dependency versions still require monitoring and maintenance.

---

# Component Provenance

Provenance answers:

> Where did this component come from?

Important questions:

- Who produced it?
- Was it altered?
- Is the source trusted?
- Is the artifact authentic?

This is important for supply-chain security.

---

# 5.6 Apply Security During the Build Process

Security continues after developers finish writing source code.

The build process creates:

> Deployable software artifacts.

The build must therefore be trusted.

---

# Secure Build Pipeline

Conceptually:

```text
Source Code
     ↓
Dependencies
     ↓
Compiler / Build Tools
     ↓
Security Analysis
     ↓
Build Artifact
     ↓
Signing
```

Each step can introduce risk.

---

# Reproducible / Controlled Builds

Build processes should use:

- Controlled tools
- Approved dependencies
- Known configurations
- Versioned build instructions

This improves:

- Repeatability
- Traceability
- Integrity

---

# Anti-Tampering Techniques

Anti-tampering techniques make unauthorized modification:

- More difficult
- More detectable

Examples:

- Code signing
- Integrity checks
- Obfuscation

---

# Code Signing

Code signing uses a digital signature to provide evidence of:

- Artifact origin
- Integrity

Conceptually:

```text
Software Artifact
      ↓
Hash
      ↓
Sign with private key
      ↓
Digital Signature
```

Users verify using:

> The corresponding public key.

---

# Code Signing Does NOT Primarily Provide

Code signing does not primarily provide:

> Confidentiality.

The program can still be readable.

Its purpose is closer to:

> Authenticity + integrity.

---

# Signing Key Protection

A code-signing system is only trustworthy if:

> The signing private key is protected.

If an attacker steals the signing key:

```text
Malicious Code
     ↓
Valid Signature
```

can appear legitimate.

---

# Obfuscation

Obfuscation makes code harder to understand or reverse engineer.

Possible uses:

- Intellectual property protection
- Increase attacker effort
- Anti-tampering

However:

> Obfuscation is not a replacement for access control, encryption, or secure coding.

---

# Compiler Security Switches

Compilers may provide security-hardening options.

Examples conceptually include:

- Stack protections
- Memory protections
- Runtime safety checks
- Position-independent execution

Secure build configurations should enable appropriate protections.

---

# Build Configuration Trap

Debug and development settings should not automatically become production settings.

Examples:

- Debug symbols
- Verbose logging
- Disabled protections
- Development credentials

---

# Compiler Warnings

Compiler warnings may indicate:

- Unsafe conversions
- Uninitialized values
- Deprecated functions
- Memory issues
- Type errors

### Exam Rule

Do not simply suppress security-relevant warnings to make the build succeed.

Instead:

> Understand and address them.

---

# Warning as Error

Some organizations treat selected warnings as build failures.

Example:

```text
Security warning detected
       ↓
Build fails
       ↓
Developer fixes issue
```

This supports:

> Secure build gates.

---

# High-Value Exam Distinctions

## Validation vs. Sanitization

```text
Validation
→ Is this data acceptable?

Sanitization
→ Transform/remove dangerous content.
```

---

## Input Validation vs. Output Encoding

```text
Input Validation
→ Protect application processing.

Output Encoding
→ Protect the destination interpreter.
```

---

## Authentication vs. Session Management

```text
Authentication
→ Establish identity.

Session Management
→ Maintain authenticated state securely.
```

---

## Tokenization vs. Encryption

```text
Tokenization
→ Replace sensitive value with surrogate.

Encryption
→ Cryptographically transform the value.
```

---

## SAST vs. SCA

```text
SAST
→ Analyze custom application code.

SCA
→ Analyze third-party components.
```

---

## SAST vs. Manual Review

```text
SAST
→ Automated pattern analysis.

Manual Review
→ Human reasoning and context.
```

---

## CWE vs. CVE

```text
CWE
→ Class of software weakness.

CVE
→ Specific publicly identified vulnerability.
```

---

## Backdoor vs. Logic Bomb

```text
Backdoor
→ Hidden access mechanism.

Logic Bomb
→ Malicious behavior triggered by a condition.
```

---

## FIM vs. Anti-Malware

```text
FIM
→ Detect unexpected file changes.

Anti-Malware
→ Detect/block malicious software.
```

---

## Code Signing vs. Encryption

```text
Code Signing
→ Authenticity + Integrity.

Encryption
→ Confidentiality.
```

---

## Obfuscation vs. Encryption

```text
Obfuscation
→ Make understanding difficult.

Encryption
→ Cryptographically prevent unauthorized reading.
```

---

## Declarative vs. Imperative Security

```text
Declarative
→ Security defined through policy/configuration.

Imperative
→ Security implemented through program logic.
```

---

## Race Condition vs. TOCTOU

```text
Race Condition
→ Result depends on competing timing.

TOCTOU
→ State changes between security check and use.
```

---

# Common CSSLP Domain 5 Exam Traps

## Trap 1 — Trusting Client-Side Validation

Wrong:

> Browser validation prevents invalid values, so the server does not need validation.

Better:

> Validate again at the trusted server boundary.

---

## Trap 2 — Denylisting Every Attack String

Wrong:

```text
Block:
<script>
SELECT
DROP
../
```

Better:

> Define permitted input whenever practical.

---

## Trap 3 — Input Validation Prevents All XSS

Wrong:

> We validated the input, therefore output encoding is unnecessary.

Better:

> Encode output for the destination context.

---

## Trap 4 — Displaying Detailed Errors

Wrong:

> Give the user the complete stack trace so troubleshooting is easier.

Better:

> Return generic user-facing errors and securely log diagnostic details.

---

## Trap 5 — Logging Everything

Wrong:

> More logs always mean better security.

Better:

> Log useful security events without exposing secrets or unnecessary personal information.

---

## Trap 6 — Reusing the Same Session After Privilege Change

Wrong:

> Administrator login uses the same session identifier that existed before authentication.

Better:

> Regenerate or rotate session context after authentication or privilege changes.

---

## Trap 7 — Internal APIs Are Trusted

Wrong:

> The API is internal, therefore input validation and authentication are unnecessary.

Better:

> Treat interfaces according to their trust boundary, not simply their network location.

---

## Trap 8 — Hard-Coded Credentials

Wrong:

```text
dbPassword = "ProductionSecret123!"
```

Better:

> Use controlled secret management.

---

## Trap 9 — Encryption Means Secure Storage

Wrong:

> Database is encrypted, therefore access controls are unnecessary.

Better:

> Encryption complements—not replaces—authorization and least privilege.

---

## Trap 10 — Build Passed, Therefore Code Is Secure

Wrong:

> Successful compilation proves the application is secure.

Better:

> Compilation verifies syntactic/build correctness, not complete security.

---

## Trap 11 — SAST Finds Everything

Wrong:

> Static analysis found zero issues, therefore security review is complete.

Better:

> Combine automated and human techniques.

---

## Trap 12 — Open Source Is Automatically Safe

Wrong:

> Thousands of developers can see the code, therefore it has no vulnerabilities.

Better:

> Evaluate version, vulnerabilities, provenance, maintenance, and dependencies.

---

## Trap 13 — Open Source Is Automatically Unsafe

Also wrong.

Open source can be secure when:

- Properly maintained
- Reviewed
- Updated
- Monitored

---

## Trap 14 — FIM Prevents Tampering

Wrong:

> File integrity monitoring prevents every unauthorized modification.

Better:

> It primarily detects changes.

---

## Trap 15 — Code Signing Encrypts the Application

Wrong:

> Signed software cannot be read.

Better:

> Signing proves origin/integrity; it does not inherently hide the software.

---

## Trap 16 — Suppress Compiler Warnings

Wrong:

> Warnings break the CI pipeline, so disable them.

Better:

> Investigate and remediate security-relevant warnings.

---

## Trap 17 — Compensating Control Equals Fix

Wrong:

> Firewall blocks exploitation, so vulnerable code never needs remediation.

Better:

> Treat compensating controls according to risk while pursuing root-cause remediation where required.

---

## Trap 18 — Component Is Secure in Isolation

Wrong:

> Both systems passed security testing individually, therefore integration is secure.

Better:

> Test assumptions and trust contracts at the integration boundary.

---

# Scenario Recognition Examples

## Scenario 1

An application accepts an age value from a web form.

JavaScript prevents numbers below 18, but modifying the HTTP request allows:

```text
age=-500
```

Think:

> **Server-side input validation**

---

## Scenario 2

An application stores comments and later displays them directly in a browser.

An attacker submits:

```text
<script>...</script>
```

Think:

> **Context-appropriate output encoding / XSS prevention**

---

## Scenario 3

Two simultaneous withdrawal requests both check the balance before either updates it.

Think:

> **Race condition / concurrency control**

---

## Scenario 4

An application checks that a temporary file is owned by the user and then opens it.

An attacker replaces the file between those operations.

Think:

> **TOCTOU**

---

## Scenario 5

A login error displays:

```text
SQLSTATE error
Database: prod-finance-02
Table: users
```

Think:

> **Improper error handling / information disclosure**

---

## Scenario 6

The application logs:

```text
User login:
username=alice
password=Secret123!
```

Think:

> **Sensitive information in logs**

---

## Scenario 7

A user authenticates and receives session identifier:

```text
100001
```

The next user receives:

```text
100002
```

Think:

> **Predictable session identifiers**

---

## Scenario 8

An application has no upload limits, allowing one user to consume all available disk storage.

Think:

> **Resource management / denial of service**

---

## Scenario 9

A service account password appears in the Git repository.

Think:

> **Credential/configuration management failure**

---

## Scenario 10

A payment card number is replaced in the application database by:

```text
TOKEN-A9182
```

while the real number is stored in a secure token vault.

Think:

> **Tokenization**

---

## Scenario 11

The organization needs to identify security weaknesses in its source code before executing the application.

Think:

> **SAST**

---

## Scenario 12

The organization needs to determine whether its application includes a vulnerable version of an open-source JSON library.

Think:

> **SCA**

---

## Scenario 13

A developer intentionally adds a hidden administrator password.

Think:

> **Backdoor**

---

## Scenario 14

Malicious code deletes data if a specific employee is terminated.

Think:

> **Logic bomb**

---

## Scenario 15

A monitor calculates hashes of critical executables and alerts when they change.

Think:

> **File Integrity Monitoring**

---

## Scenario 16

A third-party module passed security testing alone but uses a different authentication assumption than the primary application.

Think:

> **Integration security / trust-contract problem**

---

## Scenario 17

A build pipeline produces an executable and digitally signs it before distribution.

Think:

> **Code signing / artifact integrity and authenticity**

---

## Scenario 18

A compiler warns about use of an unsafe memory function.

The development team disables the warning.

Think:

> **Insecure build practice**

Better:

> Investigate and remediate the code.

---

# Secure Coding Decision Flow

When handling untrusted data:

```text
Receive Input
     ↓
Canonicalize if needed
     ↓
Validate
     ↓
Authorize Operation
     ↓
Process Safely
     ↓
Encode for Output Context
     ↓
Log Appropriate Security Event
```

---

# Dependency Security Flow

```text
Select Component
      ↓
Verify Provenance
      ↓
Identify Version
      ↓
Run SCA
      ↓
Review Known Vulnerabilities
      ↓
Evaluate License / Maintenance
      ↓
Integrate
      ↓
Security Test
      ↓
Continuously Monitor
```

---

# Secure Build Flow

```text
Trusted Source
     ↓
Approved Dependencies
     ↓
Secure Compiler Settings
     ↓
Address Warnings
     ↓
SAST / Security Gates
     ↓
Build Artifact
     ↓
Integrity Verification
     ↓
Code Signing
```

---

# Security Review Checklist for Implementation

```text
[ ] Is all untrusted input validated?

[ ] Is output encoded for the correct context?

[ ] Are parameterized queries used?

[ ] Are errors handled securely?

[ ] Are secrets excluded from logs?

[ ] Are sessions unpredictable and revocable?

[ ] Are credentials stored outside source code?

[ ] Are resource limits enforced?

[ ] Are secure defaults used?

[ ] Are cryptographic algorithms approved?

[ ] Is crypto agility considered?

[ ] Is access control enforced server-side?

[ ] Are third-party libraries inventoried?

[ ] Are dependencies scanned?

[ ] Has SAST been performed?

[ ] Has manual review covered sensitive code?

[ ] Are malicious-code indicators reviewed?

[ ] Are integration trust assumptions tested?

[ ] Are compiler protections enabled?

[ ] Are compiler warnings addressed?

[ ] Are build artifacts signed or otherwise integrity-protected?
```

---

# Fast Memory Sheet

```text
DECLARATIVE SECURITY
→ Define security as policy/configuration.

IMPERATIVE SECURITY
→ Enforce security in program logic.

CONCURRENCY
→ Safely coordinate simultaneous operations.

RACE CONDITION
→ Result depends on timing.

TOCTOU
→ State changes between check and use.

INPUT VALIDATION
→ Accept only valid data.

SANITIZATION
→ Transform/remove dangerous content.

OUTPUT ENCODING
→ Keep data from becoming executable syntax.

ERROR HANDLING
→ Fail safely and hide sensitive details.

SECURE LOGGING
→ Record useful events without leaking secrets.

SESSION MANAGEMENT
→ Protect authenticated state.

RESOURCE MANAGEMENT
→ Limit CPU, memory, storage, and connections.

SECURE CONFIGURATION
→ Safe defaults and protected credentials.

TOKENIZATION
→ Replace sensitive data with surrogate values.

ISOLATION
→ Limit blast radius.

CRYPTO AGILITY
→ Allow algorithms to be replaced.

RBAC
→ Permissions through roles.

DAC
→ Owner controls access.

MAC
→ Centrally enforced labels.

SAST
→ Analyze code without running it.

MANUAL REVIEW
→ Human security analysis.

CWE
→ Weakness category.

CVE
→ Specific vulnerability.

SCA
→ Analyze third-party dependencies.

BACKDOOR
→ Hidden access path.

LOGIC BOMB
→ Malicious action triggered by condition.

FIM
→ Detect file changes.

WATCHDOG
→ Detect process/system failure.

CODE SIGNING
→ Artifact authenticity + integrity.

COMPILER SWITCHES
→ Enable platform/build protections.

COMPILER WARNINGS
→ Investigate, don't blindly suppress.
```

---

# Hardest Domain 5 Distinctions to Memorize

| Question Is Asking... | Likely Concept |
|---|---|
| Define security through configuration? | **Declarative Security** |
| Security implemented directly in code? | **Imperative Security** |
| Two operations interfere because of timing? | **Race Condition** |
| Resource changed after security check? | **TOCTOU** |
| Is this incoming value acceptable? | **Input Validation** |
| Make data safe for browser/SQL/etc.? | **Output Encoding / Parameterization** |
| Replace sensitive value with surrogate? | **Tokenization** |
| Prevent compromised component reaching others? | **Isolation** |
| Replace cryptographic algorithm later? | **Crypto Agility** |
| Analyze source automatically? | **SAST** |
| Identify vulnerable dependency? | **SCA** |
| Classify type of coding weakness? | **CWE** |
| Identify specific published vulnerability? | **CVE** |
| Hidden unauthorized access functionality? | **Backdoor** |
| Malicious code activates on trigger? | **Logic Bomb** |
| Detect unauthorized file change? | **FIM** |
| Verify software producer and integrity? | **Code Signing** |
| Security option provided by compiler? | **Compiler Switch** |
| Build emits security warning? | **Investigate and remediate** |

---

# CSSLP Domain 5 Exam Strategy

Domain 5 often presents a vulnerable implementation and asks for the **BEST** control.

Think in this order:

```text
Root Cause
    ↓
Prevent
    ↓
Limit Impact
    ↓
Detect
    ↓
Respond
```

When possible, prevention is usually preferable to merely detecting exploitation later.

---

# Example

Problem:

> User input is concatenated directly into SQL.

Possible answers:

A. Add more database logging  
B. Install antivirus  
C. Use parameterized queries  
D. Encrypt the database  

Best:

> **C**

Why?

Because it directly addresses the implementation weakness.

---

# Another Example

Problem:

> Application displays untrusted user data inside HTML.

Possible answers:

A. Hash the data  
B. HTML-encode output  
C. Increase password length  
D. Encrypt the database  

Best:

> **B**

Again:

> Fix the problem at its source.

---

# Prevention vs. Detection

Example:

```text
SQL Injection
```

Preventive:

> Parameterized queries.

Detective:

> Logging suspicious database errors.

The preventive control more directly addresses the vulnerability.

---

# Root Cause vs. Compensating Control

When two answers appear correct:

Prefer:

> Correcting the underlying implementation defect

over:

> Adding an unrelated external control

unless the question specifically asks for:

- Temporary mitigation
- Compensating control
- Detection

---

# FIRST Exam Logic

If the question asks what to do **FIRST** after discovering suspicious code:

Do not automatically:

- Delete it
- Deploy it
- Ignore it

First determine:

- What it does
- Whether it is authorized
- What risk it creates

Then follow the appropriate security and incident process.

---

# Build Security Principle

Never assume:

```text
Source Code Secure
=
Final Artifact Secure
```

The build process itself can introduce risk through:

- Malicious tools
- Compromised dependencies
- Insecure compiler options
- Tampering
- Stolen signing keys

Therefore protect:

> **Source + dependencies + toolchain + build + artifact**

---

# Domain 5 Master Sequence

Memorize:

```text
Secure Standard
      ↓
Secure Code
      ↓
Validate Input
      ↓
Handle Output Safely
      ↓
Protect Sessions / Secrets
      ↓
Analyze Code
      ↓
Analyze Dependencies
      ↓
Implement Controls
      ↓
Treat Risk
      ↓
Integrate Components
      ↓
Secure Build
      ↓
Sign Artifact
```

---

# Final Exam Rules

### Rule 1

> Validate untrusted input at the trusted boundary.

### Rule 2

> Prefer allowlisting valid input when practical.

### Rule 3

> Output encoding depends on the destination context.

### Rule 4

> Use parameterized queries rather than concatenating untrusted SQL input.

### Rule 5

> Error messages should not disclose sensitive implementation details.

### Rule 6

> Logs must support accountability without becoming a repository of secrets.

### Rule 7

> Rotate or re-establish session context after authentication and important privilege changes.

### Rule 8

> Apply resource limits to prevent exhaustion attacks.

### Rule 9

> Credentials and cryptographic keys should not be hard-coded in source code.

### Rule 10

> Tokenization and encryption are different controls.

### Rule 11

> Use approved cryptography and design for cryptographic agility.

### Rule 12

> Authorization must be enforced at the trusted/server side.

### Rule 13

> SAST analyzes code; SCA analyzes components and dependencies.

### Rule 14

> Automated analysis does not replace human review.

### Rule 15

> CWE describes weakness classes; CVE identifies specific vulnerabilities.

### Rule 16

> High entropy is an indicator requiring analysis, not automatic proof of malicious code.

### Rule 17

> Third-party and open-source components require security evaluation.

### Rule 18

> Integration can create vulnerabilities even when individual components are secure.

### Rule 19

> Code signing provides authenticity and integrity, not confidentiality.

### Rule 20

> Compiler protections should be enabled where appropriate, and security-relevant warnings should be investigated rather than suppressed.

### Rule 21

> If two answers seem correct, choose the one that most directly addresses the implementation root cause.

---

## Disclaimer

This is an independent CSSLP study guide and is not an official ISC2 publication or a collection of official exam questions.
