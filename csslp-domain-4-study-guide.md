# CSSLP Domain 4 Study Guide
## Secure Software Architecture and Design

This study guide focuses on **CSSLP Domain 4: Secure Software Architecture and Design**.

Domain 4 is highly scenario-oriented. The exam frequently expects you to identify the architecture or design decision that **BEST reduces risk before implementation begins**.

Key areas include:

1. Security architecture
2. Secure interface design
3. Reusable security technologies
4. Threat modeling
5. Architectural risk assessment and design reviews
6. Non-functional security properties and constraints
7. Secure operational architecture

---

# Domain 4 Mental Model

Think of Domain 4 as:

```text
Requirements
     ↓
Security Architecture
     ↓
Trust Boundaries
     ↓
Interfaces
     ↓
Threat Modeling
     ↓
Security Controls
     ↓
Architecture Review
     ↓
Operational Architecture
```

Domain 3 answers:

> What security does the system require?

Domain 4 answers:

> How should the system be structured to satisfy those requirements securely?

---

# Fast Exam Recognition Table

| If You See... | Think... |
|---|---|
| High-level security structure | **Security Architecture** |
| Business-driven architecture framework | **SABSA** |
| Authentication delegated across organizations | **Federated Identity** |
| Multiple architectural security safeguards | **Security Controls** |
| Client talks to centralized server | **Client/Server** |
| Nodes communicate directly | **Peer-to-Peer** |
| Components communicate asynchronously | **Message Queue** |
| Presentation, logic, and data separated | **N-Tier Architecture** |
| Independent services communicating over interfaces | **SOA / Microservices** |
| Central integration bus | **Enterprise Service Bus** |
| Browser/client performs significant processing | **Rich Internet Application** |
| Sensors, RFID, NFC, smart devices | **IoT / Ubiquitous Computing** |
| Device verifies software before startup | **Secure Boot** |
| Device firmware must be updated safely | **Secure Update** |
| SaaS / PaaS / IaaS | **Cloud Architecture** |
| Mobile app silently collects location/contact data | **Privacy / Implicit Data Collection** |
| Timing/cache/power information leaks secrets | **Side-Channel Attack** |
| CPU execution optimization creates exposure | **Speculative Execution Risk** |
| Hardware-protected secret storage | **Secure Element / TPM** |
| Administrative interface | **Management Interface Security** |
| Separate management network | **Out-of-Band Management** |
| API security decisions | **Secure Interface Design** |
| Dependencies between systems | **Upstream / Downstream Dependencies** |
| Certificates / SSO | **Credential Management** |
| Proxy, firewall, queuing | **Flow Control** |
| Sensitive data leaving organization | **DLP** |
| Hypervisor / containers | **Virtualization** |
| TPM / TCB | **Trusted Computing** |
| Database views and privileges | **Database Security** |
| STRIDE | **Threat Categorization** |
| Business-driven attack simulation | **PASTA** |
| Everything reachable by attackers | **Attack Surface** |
| Identify attacker capabilities and targets | **Threat Analysis** |
| Current information about adversaries | **Threat Intelligence** |
| Review architecture before implementation | **Design Review** |
| Availability, reliability, scalability constraints | **Non-Functional Properties** |
| Production topology / CI/CD interfaces | **Secure Operational Architecture** |

---

# 4.1 Define the Security Architecture

Security architecture describes the high-level security structure of a system.

It identifies:

- Trust boundaries
- Components
- Security controls
- Data flows
- Authentication mechanisms
- Authorization mechanisms
- External dependencies
- Communication paths
- Security zones

A simplified model:

```text
Internet
   ↓
[Web Tier]
   ↓
[Application Tier]
   ↓
[Database Tier]
```

Security architecture asks:

> Where should trust boundaries and security controls exist?

---

# Architecture vs. Design

These terms can overlap, but a useful exam distinction is:

## Architecture

Describes the high-level structure.

Examples:

- Tiers
- Services
- Trust boundaries
- Major components
- Communication relationships

Think:

> **How is the system structured?**

---

## Design

Describes more specific implementation approaches within that architecture.

Examples:

- Authentication flow
- API authorization model
- Session architecture
- Encryption placement
- Interface behavior

Think:

> **How will components securely interact?**

---

# Security Architecture Goal

The goal is not:

> Add security controls after building the system.

The goal is:

> Design the system so that security is inherent in its structure.

---

# SABSA

**Sherwood Applied Business Security Architecture (SABSA)** is a business-driven security architecture methodology.

A key principle is that security architecture should trace back to:

> **Business requirements and business risk.**

Think:

```text
Business Requirements
        ↓
Security Requirements
        ↓
Security Architecture
        ↓
Security Services
        ↓
Technology
```

---

# SABSA Exam Clue

If the question emphasizes:

- Business requirements
- Business risk
- Traceability from business goals to security architecture

Think:

> **SABSA**

---

# Security Chain of Responsibility

Security responsibility should be clearly assigned across components and actors.

Example:

```text
Identity Provider
      ↓
API Gateway
      ↓
Application
      ↓
Database
```

Each component has a specific responsibility.

For example:

- Identity provider → Authenticate
- API gateway → Validate tokens / filter requests
- Application → Authorize business actions
- Database → Enforce data permissions

### Exam Rule

Do not assume that because one layer performs security, every downstream component can blindly trust all requests.

---

# Federated Identity

Federated identity allows one organization or system to rely on identity assertions from another trusted identity provider.

Example:

```text
User
 ↓
Company Identity Provider
 ↓
Authentication Assertion
 ↓
External SaaS Application
```

The SaaS application may trust the identity provider for authentication while still making its own authorization decisions.

### Memory Phrase

```text
Federation
→ Authentication can be delegated.

Authorization
→ Usually remains the responsibility of the resource owner.
```

---

# Security Controls Identification

During architecture, identify controls needed to address:

- Confidentiality
- Integrity
- Availability
- Authentication
- Authorization
- Accountability
- Privacy
- Nonrepudiation

Controls should be selected based on:

> **Risk and security requirements**

—not merely because a technology is popular.

---

# Control Prioritization

Controls should generally be prioritized according to:

```text
Risk
+
Business Impact
+
Threat Likelihood
+
Security Requirements
```

### Exam Trap

Do not select controls primarily because they are:

- Cheapest
- Newest
- Most technically advanced
- Easiest to deploy

The best control addresses the relevant risk.

---

# Distributed Computing

Distributed systems have components running across multiple systems or locations.

Examples include:

- Client/server
- Peer-to-peer
- Message queues
- N-tier systems

Distributed architecture introduces security concerns such as:

- Trust between nodes
- Network interception
- Authentication
- Authorization
- Data consistency
- Availability
- Message integrity

---

# Client/Server

A centralized server provides services to multiple clients.

```text
Client
   ↓
Server
   ↓
Database
```

Security considerations include:

- Client trust
- Server authentication
- Session security
- Network encryption
- Input validation

### Important

Never assume:

> The client is trusted because the organization wrote it.

Attackers can manipulate clients.

---

# Peer-to-Peer — P2P

In P2P architecture, nodes communicate directly.

```text
Node A ↔ Node B
  ↕       ↕
Node C ↔ Node D
```

Security concerns include:

- Trust between peers
- Malicious nodes
- Data integrity
- Authentication
- Distributed availability

---

# Message Queues

Message queues allow components to communicate asynchronously.

```text
Producer
   ↓
Queue
   ↓
Consumer
```

Benefits include:

- Decoupling
- Resilience
- Scalability

Security concerns include:

- Unauthorized producers
- Unauthorized consumers
- Message tampering
- Sensitive data in queues
- Replay
- Queue exhaustion

---

# N-Tier Architecture

N-tier architecture separates functionality into layers.

Example:

```text
Presentation Tier
       ↓
Application Tier
       ↓
Data Tier
```

Security benefits can include:

- Isolation
- Smaller trust zones
- Separation of responsibilities
- Defense in depth

### Exam Clue

Separating the web server from the database is usually preferable to placing everything in one unrestricted tier.

---

# Service-Oriented Architecture — SOA

SOA organizes functionality into reusable services.

Services communicate through defined interfaces.

Security concerns include:

- Service authentication
- Authorization
- Message integrity
- Service discovery
- Trust relationships
- Interface exposure

---

# Enterprise Service Bus — ESB

An ESB can act as a centralized integration layer between services.

```text
Service A
    \
Service B → ESB → Service C
    /
Service D
```

Potential benefits:

- Centralized integration
- Routing
- Transformation
- Policy enforcement

Potential risk:

> The ESB can become a high-value target or critical dependency.

---

# Microservices

Microservices break applications into smaller independently deployed services.

```text
Authentication Service
Payment Service
Order Service
Inventory Service
Notification Service
```

Security concerns include:

- Service-to-service authentication
- API authorization
- Secrets
- Network exposure
- Logging
- Distributed trust
- Increased attack surface

### Important Exam Point

Microservices may reduce monolithic coupling but increase:

> **Number of interfaces and trust relationships**

---

# Monolith vs. Microservices

```text
Monolith
→ Fewer internal network interfaces
→ Larger single application boundary

Microservices
→ Smaller individual services
→ More APIs and trust boundaries
```

Neither is automatically more secure.

Security depends on architecture and controls.

---

# Rich Internet Applications

Rich Internet Applications perform significant client-side processing.

Security concerns include:

- Client-side manipulation
- Untrusted browser environment
- Remote code execution
- Continuous connectivity
- Client-side storage

### Core Rule

> **Never trust client-side enforcement alone.**

Attackers control the client environment.

---

# Example

Bad:

```text
JavaScript:
if user.role == "admin":
    showAdminButton()
```

If the server relies on this check, the attacker may bypass it.

Correct approach:

> Server-side authorization must independently enforce access.

---

# Pervasive / Ubiquitous Computing

Examples include:

- IoT
- Wireless devices
- Sensors
- RFID
- NFC
- Mesh networks
- Location-aware devices

Security challenges include:

- Limited processing capacity
- Limited update capability
- Physical exposure
- Long device lifetime
- Privacy
- Weak default credentials
- Untrusted networks

---

# RFID

**Radio-Frequency Identification**

Used for:

- Asset tracking
- Access badges
- Inventory
- Identification

Potential threats:

- Unauthorized scanning
- Cloning
- Tracking
- Replay

---

# NFC

**Near Field Communication**

Used for:

- Contactless payments
- Device pairing
- Access systems

Security concerns include:

- Relay attacks
- Eavesdropping
- Malicious tags
- Unauthorized transactions

---

# Embedded Software

Embedded systems often have security constraints not found in traditional applications.

Examples:

- Medical devices
- Vehicles
- Appliances
- Industrial controllers
- IoT devices

Important concepts include:

- Secure boot
- Secure memory
- Secure update

---

# Secure Boot

Secure boot helps ensure that a device executes authorized software during startup.

Simplified concept:

```text
Boot ROM
   ↓ verifies
Bootloader
   ↓ verifies
Operating System / Firmware
```

This creates a:

> **Chain of trust**

---

# Secure Update

Updates should provide assurance of:

- Authenticity
- Integrity
- Authorized origin
- Version validity

Typical protection:

```text
Firmware
   ↓
Digital Signature
   ↓
Device Verifies Signature
   ↓
Install
```

### Exam Trap

Encryption alone does not prove that firmware came from the trusted manufacturer.

Digital signatures are more relevant to authenticity and integrity.

---

# Secure Memory

Embedded systems may require protection for:

- Keys
- Credentials
- Firmware
- Sensitive runtime data

Potential mechanisms include:

- Memory isolation
- Secure elements
- Hardware-backed storage
- Access controls

---

# Cloud Architecture

Cloud service models commonly include:

- SaaS
- PaaS
- IaaS

---

# SaaS

**Software as a Service**

Provider manages most of the application stack.

Customer primarily uses and configures the application.

Examples of customer responsibilities may include:

- Identity
- User access
- Data configuration
- Security settings

---

# PaaS

**Platform as a Service**

Provider manages underlying infrastructure and platform services.

Customer typically manages:

- Application code
- Application configuration
- Data
- Identity

---

# IaaS

**Infrastructure as a Service**

Provider manages physical infrastructure and virtualization.

Customer typically manages more of:

- Operating systems
- Applications
- Configuration
- Data
- Patching

---

# Shared Responsibility

A crucial cloud concept:

> Moving to the cloud does not remove security responsibility.

Responsibilities depend on the service model.

```text
SaaS → Provider manages more
PaaS → Shared middle ground
IaaS → Customer manages more
```

---

# Mobile Applications

Mobile apps introduce concerns including:

- Device loss
- Local storage
- Permission abuse
- Location tracking
- Sensors
- Camera
- Microphone
- Contacts
- Clipboard
- Background collection

---

# Implicit Data Collection

A mobile application may collect data users do not realize is being collected.

Examples:

- Location
- Device identifiers
- Contact lists
- Usage patterns
- Sensor data

### Exam Principle

Use:

> Data minimization and explicit privacy-aware design.

---

# Hardware Platform Concerns

Architecture must sometimes account for hardware-level security.

Examples include:

- Side-channel attacks
- Speculative execution
- Secure elements
- Firmware
- Drivers

---

# Side-Channel Attacks

A side-channel attack obtains information indirectly from physical or operational behavior.

Examples:

- Timing
- Power consumption
- Electromagnetic emissions
- Cache behavior

Conceptually:

```text
Cryptographic Operation
        ↓
Observable Behavior
        ↓
Attacker Infers Secret
```

---

# Speculative Execution

Modern processors predict and execute instructions ahead of time for performance.

Security vulnerabilities may allow attackers to infer information through:

- Cache state
- Timing
- Microarchitectural behavior

### Exam Clue

If a vulnerability arises from:

> CPU optimization leaking memory across boundaries

Think:

> **Speculative execution / microarchitectural risk**

---

# Secure Element

A secure element is a protected hardware component designed to store and process sensitive material.

Examples:

- Cryptographic keys
- Payment credentials
- Device identity

Think:

> **Hardware-isolated secret protection**

---

# Firmware and Drivers

Firmware and drivers operate at highly privileged levels.

Therefore vulnerabilities can have significant impact.

Architecture should consider:

- Signed updates
- Secure boot
- Least privilege
- Update mechanisms
- Driver trust

---

# Cognitive Computing

Current architecture considerations may include technologies such as:

- Artificial intelligence
- Machine learning
- Virtual reality
- Augmented reality

Security concerns can include:

- Sensitive training/input data
- Trust boundaries
- Manipulated inputs
- Model access
- Data leakage
- Unpredictable outputs

The architecture principle remains:

> Treat externally influenced components and outputs according to their trust level.

---

# Industrial IoT

Industrial IoT examples include:

- Manufacturing
- Automotive systems
- Robotics
- Medical devices
- Building management
- Industrial control systems

Security priorities often include:

- Safety
- Availability
- Integrity
- Resilience
- Secure updates

### Exam Point

In industrial systems:

> Availability and safety may have direct physical consequences.

---

# 4.2 Perform Secure Interface Design

Every interface creates a potential trust boundary.

Examples include:

- APIs
- Administrative consoles
- Logging interfaces
- Database interfaces
- Service interfaces
- Management networks

---

# Interface Security Questions

For every interface, ask:

```text
Who can call it?
      ↓
How are they authenticated?
      ↓
What are they authorized to do?
      ↓
What data crosses it?
      ↓
How is the data protected?
      ↓
What happens if the interface fails?
```

---

# Security Management Interfaces

Administrative interfaces are particularly sensitive because they often provide:

- Configuration access
- User administration
- Security policy changes
- System control

Best practices include:

- Strong authentication
- Least privilege
- Network restriction
- Audit logging
- MFA
- Dedicated management networks

---

# Out-of-Band Management

Out-of-band management uses a management path separate from the normal production network.

```text
Production Network
       |
   Application

Separate Management Network
       |
   Admin Interface
```

Benefits include:

- Isolation
- Reduced attack surface
- Management during production network failure

### Exam Clue

If administrators should manage critical infrastructure without exposing management interfaces to normal users:

> **Out-of-band management**

---

# Logging Interfaces

Logging interfaces should be designed securely because logs may contain:

- User identifiers
- Error details
- Tokens
- System information
- Security events

Risks include:

- Sensitive-data leakage
- Log injection
- Unauthorized log modification
- Denial of logging capacity

---

# Upstream and Downstream Dependencies

Systems rarely operate independently.

Example:

```text
Identity Provider
     ↓
Application
     ↓
Payment Processor
     ↓
Bank
```

An upstream or downstream failure can affect your security.

Questions to ask:

- What data is shared?
- What keys are shared?
- Which system is trusted?
- How are failures handled?
- What if the dependency is compromised?

---

# Key Sharing Between Applications

Sharing cryptographic keys across many applications increases risk.

If one application is compromised:

> Other systems using the same key may also be affected.

Prefer:

- Separate keys
- Defined ownership
- Rotation
- Proper key lifecycle management

---

# Protocol Design

Protocol design defines how systems communicate.

Security considerations include:

- Authentication
- Confidentiality
- Integrity
- Replay resistance
- State
- Error behavior
- Version negotiation

---

# Stateful vs. Stateless

## Stateful

Server maintains context between requests.

Example:

```text
Login
 ↓
Server Session Created
 ↓
Subsequent Requests Use Session
```

Security concerns:

- Session theft
- Session fixation
- Expiration
- Revocation

---

## Stateless

Each request contains the information required to process it.

Example:

```text
Request + Signed Token
        ↓
Server validates token
```

Security concerns:

- Token theft
- Token expiration
- Revocation complexity
- Replay

### Exam Rule

Neither model is inherently secure.

Security depends on correct design.

---

# API Architecture

API security should consider:

- Authentication
- Authorization
- Object-level permissions
- Rate limiting
- Input validation
- Encryption
- Logging
- Versioning

### Critical Rule

> Authentication does not replace authorization.

---

# 4.3 Evaluate and Select Reusable Technologies

Domain 4 expects architects to evaluate existing technologies instead of unnecessarily reinventing security mechanisms.

Areas include:

- Credential management
- Flow control
- DLP
- Virtualization
- Trusted computing
- Database security
- Runtime environments
- Operating system controls
- Backup
- Retention and destruction

---

# Credential Management

Credential mechanisms may include:

- X.509 certificates
- SSO
- Tokens
- Hardware-backed credentials

---

# X.509

X.509 certificates are commonly used in PKI.

They can bind:

```text
Identity
   +
Public Key
   ↓
Digitally Signed Certificate
```

Common uses:

- TLS
- Client authentication
- Code signing
- Device identity

---

# SSO

**Single Sign-On**

Allows a user to authenticate once and access multiple systems.

Advantages:

- Reduced password burden
- Centralized identity policy
- Simplified access management

Risk:

> Compromise of the central identity may affect many applications.

Therefore SSO should have strong protection.

---

# Flow Control

Flow control technologies regulate communication between components.

Examples:

- Proxies
- Firewalls
- Protocol controls
- Message queues

---

# Proxy

A proxy sits between communicating parties.

```text
Client
  ↓
Proxy
  ↓
Server
```

Potential functions:

- Filtering
- Authentication
- Inspection
- Logging
- Network isolation

---

# Firewall

A firewall controls network communication according to defined rules.

It is useful for:

- Network segmentation
- Limiting exposed services
- Enforcing communication paths

### Exam Trap

A firewall does not replace application-layer authorization.

---

# Data Loss Prevention — DLP

DLP aims to detect or prevent unauthorized movement of sensitive information.

Possible locations:

- Endpoint
- Network
- Cloud
- Email

Think:

> **Prevent sensitive information from leaving approved boundaries.**

---

# Virtualization

Virtualization creates logical isolation between workloads.

Examples:

- Virtual machines
- Hypervisors
- Containers

---

# Hypervisor

A hypervisor manages virtual machines.

Because it controls isolation:

> Hypervisor compromise may affect multiple guests.

Therefore it is highly security-sensitive.

---

# Containers

Containers share more of the host operating system than traditional VMs.

Advantages:

- Lightweight
- Rapid deployment

Security considerations:

- Namespace isolation
- Host kernel
- Container privileges
- Image security

### Exam Rule

Containers are isolation mechanisms, but:

> They should not automatically be considered equivalent to strong hardware separation.

---

# Infrastructure as Code — IaC

IaC defines infrastructure configuration using version-controlled code.

Examples:

```text
Network
Firewall
Virtual Machines
Cloud Resources
Permissions
```

Benefits include:

- Repeatability
- Auditability
- Consistency
- Automated deployment

Risks include:

- Hard-coded secrets
- Insecure templates
- Excessive permissions

---

# Trusted Computing

Trusted computing uses hardware and software mechanisms to establish trustworthy system state.

Important concepts include:

- TPM
- TCB

---

# Trusted Platform Module — TPM

A TPM is a hardware security component that can support:

- Key protection
- Secure boot measurements
- Device identity
- Platform attestation

Think:

> **Hardware-backed trust and cryptographic protection**

---

# Trusted Computing Base — TCB

The TCB consists of components critical to enforcing the system's security policy.

Think:

> **Everything that must work correctly for security to hold.**

### Exam Principle

A smaller TCB is generally easier to:

- Review
- Test
- Trust

---

# Database Security

Database security can include:

- Encryption
- Privilege management
- Views
- Triggers
- Secure connections

---

# Database Views

Views can limit what users see.

Example:

Employee table:

```text
Name
Address
Salary
National ID
```

HR view:

```text
Name
Address
Salary
National ID
```

Manager view:

```text
Name
Salary
```

Views can support:

> **Least privilege and data minimization**

---

# Database Privilege Management

Database accounts should have only required permissions.

Bad:

```text
Application account → DBA
```

Better:

```text
Application account
→ SELECT on required tables
→ INSERT on required tables
```

---

# Database Encryption

May protect:

- Data at rest
- Individual columns
- Backups
- Connections

Architecture should identify:

> Where encryption is required and where keys are managed.

---

# Programming Language Environments

Examples include:

- JVM
- .NET runtime
- Python runtime
- PowerShell

Security concerns include:

- Runtime permissions
- Dependency security
- Sandboxing
- Memory safety
- Execution policies

---

# Operating System Controls

OS controls may include:

- Permissions
- Process isolation
- Memory protection
- Mandatory access control
- Logging
- Patch management

Architecture should make use of platform security rather than unnecessarily recreating it.

---

# Secure Backup and Restoration

Backup architecture must consider:

- Confidentiality
- Integrity
- Availability
- Recovery
- Access control

A backup is useful only if:

> **It can be successfully restored.**

Therefore restoration testing is essential.

---

# Backup Security

Backups may contain the same sensitive information as production.

Therefore:

> Backups require equivalent protection appropriate to their sensitivity.

---

# Secure Data Retention

Architecture should support retention requirements.

Questions include:

- What data?
- How long?
- Where?
- Who can access it?
- When must it be destroyed?

---

# Secure Destruction

Data should be destroyed when:

- Retention expires
- Legal requirements permit
- Business need ends

Secure destruction should account for:

- Production data
- Backups
- Replicas
- Cached copies

---

# 4.4 Perform Threat Modeling

Threat modeling identifies potential threats during architecture and design.

The goal is:

> **Find security problems before they become code.**

---

# Threat Modeling Workflow

```text
Understand System
      ↓
Identify Assets
      ↓
Identify Trust Boundaries
      ↓
Identify Threats
      ↓
Assess Risk
      ↓
Define Mitigations
      ↓
Review
```

---

# Data Flow Diagrams — DFD

Threat modeling often uses data flow diagrams.

Typical elements:

- External entities
- Processes
- Data stores
- Data flows
- Trust boundaries

Example:

```text
User
 ↓
[Web Application]
 ↓
---------------- Trust Boundary ----------------
 ↓
[Database]
```

Trust boundaries are particularly important because:

> Data crossing a trust boundary requires security consideration.

---

# STRIDE

STRIDE is a threat-modeling mnemonic.

```text
S → Spoofing
T → Tampering
R → Repudiation
I → Information Disclosure
D → Denial of Service
E → Elevation of Privilege
```

---

# STRIDE Mapping

| STRIDE Threat | Security Property |
|---|---|
| **Spoofing** | Authentication |
| **Tampering** | Integrity |
| **Repudiation** | Accountability / Nonrepudiation |
| **Information Disclosure** | Confidentiality |
| **Denial of Service** | Availability |
| **Elevation of Privilege** | Authorization |

This table is extremely useful for the exam.

---

# Spoofing

Attacker impersonates another identity.

Think:

> Authentication problem.

Example:

> Attacker uses stolen credentials.

---

# Tampering

Unauthorized modification of information.

Think:

> Integrity problem.

Example:

> Attacker changes transaction amount.

---

# Repudiation

Actor denies performing an action.

Think:

> Logging, accountability, nonrepudiation.

---

# Information Disclosure

Unauthorized exposure of information.

Think:

> Confidentiality.

---

# Denial of Service

Attacker prevents legitimate use.

Think:

> Availability.

---

# Elevation of Privilege

Attacker gains permissions beyond what was authorized.

Think:

> Authorization / privilege problem.

---

# STRIDE Memory Shortcut

```text
S → Who are you?
T → Was it changed?
R → Who did it?
I → Who can see it?
D → Can we use it?
E → What can you do?
```

---

# PASTA

**Process for Attack Simulation and Threat Analysis**

PASTA is a risk-centric threat modeling methodology.

It generally emphasizes:

- Business objectives
- Technical scope
- Threat analysis
- Vulnerability analysis
- Attack modeling
- Risk

Think:

> **Business-risk-driven attack simulation**

---

# STRIDE vs. PASTA

```text
STRIDE
→ Categorize threats.

PASTA
→ Risk-centric attack analysis and simulation.
```

If the exam emphasizes categorizing threats such as spoofing and tampering:

> STRIDE

If it emphasizes business risk and attack simulation:

> PASTA

---

# CVSS

**Common Vulnerability Scoring System**

CVSS scores vulnerability severity.

It can help prioritize known vulnerabilities.

### Important Distinction

CVSS is not a complete business-risk calculation.

A high CVSS vulnerability on an isolated test system may present less business risk than a moderate vulnerability on a critical payment platform.

---

# Attack Surface

The attack surface consists of points an attacker can potentially interact with.

Examples:

- APIs
- Open ports
- Login forms
- Upload interfaces
- Administrative consoles
- Third-party connections

Think:

> **Everything exposed that could potentially be attacked.**

---

# Attack Surface Reduction

Methods include:

- Remove unused services
- Disable unnecessary interfaces
- Restrict network exposure
- Reduce privileges
- Minimize APIs

### Exam Rule

An unused feature that is enabled still increases attack surface.

---

# Threat Analysis

Threat analysis identifies:

- Threat actors
- Capabilities
- Motivation
- Targets
- Attack paths

Example:

```text
Threat Actor
    ↓
Capability
    ↓
Attack Path
    ↓
Asset
    ↓
Impact
```

---

# Common Threat Actors

Examples include:

- External attackers
- Insiders
- Organized criminal groups
- APT actors
- Third-party suppliers
- Malware

---

# Advanced Persistent Threat — APT

An APT typically involves:

- Skilled adversaries
- Long-term persistence
- Specific objectives
- Sophisticated methods

### Exam Trap

APT does not simply mean:

> Any malware infection.

---

# Insider Threat

An insider has legitimate access or organizational proximity.

Could include:

- Malicious employee
- Negligent employee
- Contractor
- Compromised employee account

Controls may include:

- Least privilege
- Segregation of duties
- Logging
- Monitoring

---

# Third-Party Threats

Suppliers may introduce:

- Software vulnerabilities
- Compromised updates
- Data exposure
- Dependency risk
- Unauthorized access

Architecture should identify:

> Trust placed in external parties.

---

# Threat Intelligence

Threat intelligence provides information about relevant threats.

Examples:

- Attacker behavior
- Campaigns
- Vulnerabilities
- Indicators
- Tactics, techniques, and procedures

Threat intelligence helps answer:

> Which threats are credible and relevant to this system?

---

# Threat Modeling vs. Vulnerability Assessment

These are different.

## Threat Modeling

Typically occurs during architecture/design.

Asks:

> What could go wrong?

---

## Vulnerability Assessment

Examines an implemented environment for known weaknesses.

Asks:

> What weaknesses currently exist?

---

# Memory Shortcut

```text
Threat Modeling
→ Predict problems.

Vulnerability Assessment
→ Find existing weaknesses.
```

---

# 4.5 Perform Architectural Risk Assessment and Design Reviews

Architecture should be reviewed before implementation becomes expensive to change.

A design review evaluates:

- Trust boundaries
- Data flows
- Security controls
- Threat model
- Dependencies
- Failure modes
- Compliance requirements

---

# Why Design Reviews Matter

Finding a flaw during design is usually preferable to finding it after production deployment.

Example:

```text
Design:
"We need authorization between services."

versus

Production:
"Every internal service trusts every request."
```

Architecture flaws often cannot be solved effectively with a simple code patch.

---

# Architectural Risk Assessment

Risk assessment asks:

```text
What can go wrong?
        ↓
How likely is it?
        ↓
What is the impact?
        ↓
What controls exist?
        ↓
What residual risk remains?
```

---

# Architectural Risk vs. Code Vulnerability

Architectural risk:

> System design creates broad trust between all internal services.

Code vulnerability:

> SQL injection in one endpoint.

Architecture-level issues often have:

> Broader systemic impact.

---

# Design Review Timing

Best:

> Before significant implementation.

Also repeat when:

- Architecture changes
- Major features are added
- Trust boundaries change
- New external dependencies are introduced

---

# Independent Review

Independent reviewers may identify assumptions the original designers overlooked.

Peer review supports:

- Open design
- Error discovery
- Reduced individual bias

---

# Residual Risk

Residual risk is:

> Risk remaining after controls are applied.

Conceptually:

```text
Inherent Risk
     ↓
Apply Controls
     ↓
Residual Risk
```

Residual risk may require:

- Additional mitigation
- Risk acceptance
- Monitoring

---

# 4.6 Model Non-Functional Security Properties and Constraints

Architecture must satisfy security properties beyond functional behavior.

Examples:

- Availability
- Reliability
- Performance
- Scalability
- Resilience
- Confidentiality
- Integrity
- Privacy

---

# Functional vs. Non-Functional

Functional:

> User can submit a payment.

Non-functional:

> Payment service must remain available 99.99% of the time.

---

# Availability

Availability means authorized users can access systems when required.

Architectural mechanisms include:

- Redundancy
- Failover
- Clustering
- Replication

---

# Reliability

Reliability means:

> The system performs correctly and consistently over time.

Reliability and availability are related but not identical.

A system may be:

> Available but producing incorrect results.

That means it is available but not reliable.

---

# Resilience

Resilience is the ability to:

- Withstand failure
- Recover from failure
- Continue important operations

Think:

> **Survive and recover.**

---

# Scalability

Scalability means the system can support increasing load without unacceptable degradation.

Common approaches:

## Vertical Scaling

Increase capacity of one system.

```text
More CPU
More RAM
```

## Horizontal Scaling

Add additional systems.

```text
Server 1
Server 2
Server 3
```

---

# Performance vs. Security

Security mechanisms may affect performance.

Architecture should balance:

- Performance
- Security requirements
- Availability
- Cost

### Exam Principle

Do not eliminate necessary security merely to improve performance.

Instead:

> Design controls efficiently while satisfying risk requirements.

---

# Constraints

Security constraints can include:

- Approved platforms
- Required cryptography
- Data residency
- Availability targets
- Legacy integration
- Regulatory requirements

Architecture must operate within these constraints.

---

# 4.7 Define Secure Operational Architecture

Secure architecture must consider how software will actually run in production.

This includes:

- Deployment topology
- Operational interfaces
- CI/CD
- Administrative access
- Network zones

---

# Deployment Topology

Topology describes where components are deployed and how they communicate.

Example:

```text
Internet
   ↓
WAF
   ↓
Load Balancer
   ↓
Web Tier
   ↓
Application Tier
   ↓
Database Tier
```

Security architecture should identify:

- Trust zones
- Firewalls
- Security groups
- Management interfaces
- Data flows

---

# Network Segmentation

Segmentation separates systems according to trust or function.

Example:

```text
Internet Zone
      ↓
DMZ
      ↓
Application Zone
      ↓
Database Zone
```

Benefits:

- Limits lateral movement
- Reduces attack surface
- Supports least privilege
- Contains compromise

---

# DMZ

A **Demilitarized Zone** contains systems exposed to less-trusted networks.

Examples:

- Public web servers
- Reverse proxies
- Gateways

The internal database should generally not be directly exposed to the Internet.

---

# Operational Interfaces

Operational interfaces include:

- Administrative consoles
- Monitoring systems
- Logging systems
- Backup systems
- Deployment systems

These interfaces often have high privilege and therefore require strong protection.

---

# CI/CD Architecture

Continuous Integration / Continuous Delivery pipelines may have privileged access to:

- Source code
- Credentials
- Build systems
- Artifacts
- Production environments

Conceptually:

```text
Source
  ↓
Build
  ↓
Test
  ↓
Package
  ↓
Deploy
```

Security architecture should consider each transition as a trust boundary.

---

# CI/CD Security Concerns

Examples:

- Pipeline credentials
- Build server compromise
- Artifact tampering
- Excessive deployment permissions
- Untrusted dependencies

### Exam Principle

A CI/CD pipeline should be treated as:

> **Security-sensitive production infrastructure**

—not merely a developer convenience.

---

# Trust Boundaries

A trust boundary exists where:

> The level of trust changes.

Examples:

```text
Internet → Web Application
User Device → API
Application → Database
Company → Cloud Provider
Service A → Service B
```

Every trust boundary should trigger questions about:

- Authentication
- Authorization
- Encryption
- Validation
- Logging

---

# High-Value Exam Distinctions

## Architecture vs. Requirements

```text
Requirements
→ What security is needed?

Architecture
→ How should the system be structured to deliver it?
```

---

## Authentication vs. Trust

Authentication proves identity.

It does not automatically mean:

> The authenticated entity should be fully trusted.

Authorization is still required.

---

## Secure Boot vs. Encryption

```text
Secure Boot
→ Ensure authorized software starts.

Encryption
→ Prevent unauthorized disclosure.
```

---

## TPM vs. TCB

```text
TPM
→ Hardware security module / root-of-trust capability.

TCB
→ Entire collection of components relied upon to enforce security.
```

---

## SaaS vs. PaaS vs. IaaS

```text
SaaS
→ Provider manages most.

PaaS
→ Customer controls application and data.

IaaS
→ Customer controls OS, application, and more of the stack.
```

---

## Stateful vs. Stateless

```text
Stateful
→ Server retains session state.

Stateless
→ Request carries needed context.
```

Neither is automatically more secure.

---

## VM vs. Container

```text
VM
→ Separate guest operating system.

Container
→ Shares host kernel.
```

Containers are generally lighter but share more underlying infrastructure.

---

## Threat vs. Vulnerability

```text
Threat
→ Something capable of causing harm.

Vulnerability
→ A weakness that may be exploited.
```

---

## Threat Modeling vs. Vulnerability Scanning

```text
Threat Modeling
→ What could go wrong with the design?

Vulnerability Scanning
→ What known weaknesses exist in the implementation?
```

---

## STRIDE vs. PASTA

```text
STRIDE
→ Threat categories.

PASTA
→ Risk-centric attack simulation.
```

---

## Attack Surface vs. Attack Vector

```text
Attack Surface
→ All potentially exposed entry points.

Attack Vector
→ Specific method/path used to attack.
```

---

## Inherent vs. Residual Risk

```text
Inherent Risk
→ Risk before controls.

Residual Risk
→ Risk remaining after controls.
```

---

## Reliability vs. Availability

```text
Availability
→ Can I access the system?

Reliability
→ Does the system operate correctly?
```

---

## Redundancy vs. Backup

```text
Redundancy
→ Maintain continued service.

Backup
→ Restore lost or corrupted data.
```

They solve different problems.

---

# Common Domain 4 Exam Traps

## Trap 1 — Trusting Internal Traffic

Wrong:

> The request came from the internal network, so it is trusted.

Better:

> Authenticate and authorize based on identity and risk, not network location alone.

---

## Trap 2 — Trusting the Client

Wrong:

> The mobile app prevents users from selecting unauthorized transactions, so server authorization is unnecessary.

Better:

> Clients are untrusted. Enforce authorization server-side.

---

## Trap 3 — Authentication Equals Authorization

Wrong:

> The API verified the user's token, therefore all requested resources are allowed.

Better:

> Authentication establishes identity; authorization determines permitted actions.

---

## Trap 4 — Microservices Are Automatically More Secure

Wrong:

> Splitting the application into 30 services improves security automatically.

Better:

> Microservices add APIs, credentials, network paths, and trust boundaries that must be secured.

---

## Trap 5 — Encryption Solves Firmware Authenticity

Wrong:

> Encrypt firmware updates to prove they came from the manufacturer.

Better:

> Use digital signatures to verify authenticity and integrity.

---

## Trap 6 — Firewall Solves Application Security

Wrong:

> The database is behind a firewall, so database privileges do not matter.

Better:

> Use defense in depth, including database-level least privilege.

---

## Trap 7 — SSO Removes Authorization Needs

Wrong:

> Users logged in through SSO can access all connected applications.

Better:

> SSO centralizes authentication; individual systems must still authorize access.

---

## Trap 8 — Threat Modeling Happens After Coding

Wrong:

> Perform threat modeling after penetration testing finds vulnerabilities.

Better:

> Threat modeling is most valuable during requirements, architecture, and design.

---

## Trap 9 — CVSS Equals Business Risk

Wrong:

> Highest CVSS score always means highest business priority.

Better:

> Technical severity must be combined with business context.

---

## Trap 10 — Backup Equals High Availability

Wrong:

> Nightly backups eliminate application downtime.

Better:

> Backups support recovery. Redundancy/failover support availability.

---

## Trap 11 — Containers Equal Complete Isolation

Wrong:

> Containers are completely independent because each has its own kernel.

Better:

> Containers commonly share the host kernel.

---

## Trap 12 — Cloud Provider Handles Everything

Wrong:

> Migrating to SaaS/IaaS removes customer security responsibility.

Better:

> Apply the shared responsibility model.

---

## Trap 13 — Internal Services Need No Authentication

Wrong:

> Microservice traffic never leaves the data center, so authentication is unnecessary.

Better:

> Protect service-to-service trust boundaries.

---

## Trap 14 — Threat Intelligence Equals Threat Modeling

Wrong:

> Threat intelligence automatically produces the system's threat model.

Better:

> Threat intelligence informs the model, but system-specific analysis is still required.

---

## Trap 15 — High Availability Means Correct Operation

Wrong:

> The application has 100% uptime, therefore it is reliable.

Better:

> A system can be continuously available while producing incorrect results.

---

# Scenario Recognition Examples

## Scenario 1

A browser disables the "Delete Account" button for normal users, but an attacker directly calls the deletion API and succeeds.

Think:

> **Client-side controls cannot replace server-side authorization.**

---

## Scenario 2

A microservice architecture has 50 internal APIs, and every service trusts requests merely because they originate from the internal network.

Think:

> **Excessive implicit trust / service-to-service authentication and authorization problem.**

---

## Scenario 3

An IoT device installs firmware without verifying who created it.

Best architectural improvement:

> **Digitally signed secure update mechanism.**

---

## Scenario 4

An architect wants to identify spoofing, tampering, information disclosure, and privilege escalation threats.

Think:

> **STRIDE**

---

## Scenario 5

A security team wants a threat-modeling approach driven by business impact and realistic attack simulation.

Think:

> **PASTA**

---

## Scenario 6

A system uses the same encryption key across ten unrelated applications.

Think:

> **Excessive common mechanism and increased blast radius.**

Better architecture:

> Separate key ownership and minimize unnecessary sharing.

---

## Scenario 7

A customer-facing web application communicates directly with the production database from the Internet-facing tier.

Better architecture:

```text
Web Tier
   ↓
Application Tier
   ↓
Database Tier
```

Think:

> **N-tier architecture / segmentation / defense in depth**

---

## Scenario 8

A critical administrative console is reachable from the public Internet.

Best architectural change:

> Restrict or isolate the management interface, potentially through a dedicated management network or out-of-band management.

---

## Scenario 9

A cloud migration team assumes the provider will patch the guest operating system on every IaaS virtual machine.

Think:

> **Shared responsibility model misunderstanding**

In IaaS, guest OS management is commonly a customer responsibility.

---

## Scenario 10

A critical application has nightly backups but cannot tolerate more than five minutes of downtime.

Think:

> Backup alone is insufficient.

Architecture requires:

> **Redundancy / failover / high availability**

---

## Scenario 11

Security analysts want to know every external entry point attackers could interact with.

Think:

> **Attack surface evaluation**

---

## Scenario 12

A payment application is technically functioning, but transactions are periodically calculated incorrectly.

The system may have:

> High availability but poor reliability.

---

# STRIDE Master Table

| Threat | Meaning | Property at Risk | Typical Control |
|---|---|---|---|
| **Spoofing** | Pretending to be someone else | Authentication | MFA, certificates |
| **Tampering** | Unauthorized modification | Integrity | Hash/MAC/signature |
| **Repudiation** | Denying an action | Accountability | Logging/signatures |
| **Information Disclosure** | Unauthorized exposure | Confidentiality | Encryption/access control |
| **Denial of Service** | Making service unavailable | Availability | Redundancy/rate limiting |
| **Elevation of Privilege** | Gaining extra permissions | Authorization | Least privilege/access control |

---

# Architecture Review Checklist

During an architecture review, consider:

```text
[ ] What are the important assets?

[ ] Where are the trust boundaries?

[ ] Which components are exposed?

[ ] How are users authenticated?

[ ] How are users authorized?

[ ] How are services authenticated?

[ ] What sensitive data moves between components?

[ ] Is data encrypted where required?

[ ] Where are cryptographic keys stored?

[ ] What third-party dependencies exist?

[ ] What happens when dependencies fail?

[ ] Are administrative interfaces isolated?

[ ] What is the attack surface?

[ ] What threats are relevant?

[ ] What security controls mitigate them?

[ ] What residual risk remains?

[ ] How will the system operate securely in production?
```

---

# Architecture Risk Workflow

```text
Identify Assets
      ↓
Map Architecture
      ↓
Identify Trust Boundaries
      ↓
Identify Threats
      ↓
Evaluate Attack Surface
      ↓
Assess Risk
      ↓
Select Controls
      ↓
Review Architecture
      ↓
Assess Residual Risk
```

---

# Fast Memory Sheet

```text
SECURITY ARCHITECTURE
→ High-level security structure.

SABSA
→ Business-driven security architecture.

FEDERATED IDENTITY
→ Delegate authentication across trust relationships.

CLIENT/SERVER
→ Central server serves clients.

P2P
→ Peers interact directly.

MESSAGE QUEUE
→ Asynchronous communication.

N-TIER
→ Separate presentation, application, and data layers.

SOA
→ Reusable services communicating through interfaces.

MICROSERVICES
→ Small independent services with many interfaces.

RICH INTERNET APPLICATION
→ Never trust client-side controls.

IOT
→ Constrained devices + physical exposure + update concerns.

SECURE BOOT
→ Run only trusted startup software.

SECURE UPDATE
→ Authenticate and verify update integrity.

CLOUD
→ Shared responsibility.

SIDE CHANNEL
→ Learn secrets from indirect observations.

SECURE ELEMENT
→ Hardware-protected secrets.

OUT-OF-BAND MANAGEMENT
→ Separate management path.

X.509
→ Certificate binds identity to public key.

DLP
→ Prevent unauthorized sensitive-data movement.

TPM
→ Hardware-backed trust and key protection.

TCB
→ Components security depends upon.

STRIDE
→ Categorize threats.

PASTA
→ Business-risk-driven attack simulation.

ATTACK SURFACE
→ Everything potentially exposed.

THREAT INTELLIGENCE
→ Information about credible threats.

ARCHITECTURE REVIEW
→ Find structural security problems early.

RESIDUAL RISK
→ Risk remaining after controls.

OPERATIONAL ARCHITECTURE
→ How the system is securely deployed and operated.
```

---

# Hardest Domain 4 Distinctions to Memorize

| Question Is Asking... | Likely Concept |
|---|---|
| Business-driven security architecture? | **SABSA** |
| Identity assertion from another organization? | **Federated Identity** |
| Separate presentation/app/database layers? | **N-Tier** |
| Asynchronous service communication? | **Message Queue** |
| Many independent API-driven services? | **Microservices** |
| Validate firmware before startup? | **Secure Boot** |
| Verify manufacturer produced firmware? | **Digital Signature / Secure Update** |
| Separate privileged management path? | **Out-of-Band Management** |
| Stop sensitive information leaving organization? | **DLP** |
| Hardware-protected device trust? | **TPM** |
| Everything relied upon for system security? | **TCB** |
| Categorize spoofing/tampering/etc.? | **STRIDE** |
| Business-risk-driven threat analysis? | **PASTA** |
| All reachable attack entry points? | **Attack Surface** |
| Relevant current adversary information? | **Threat Intelligence** |
| Evaluate system structure before coding? | **Architectural Design Review** |
| Risk remaining after mitigation? | **Residual Risk** |
| Continue service despite component failure? | **Resilience / Redundancy** |
| Restore lost data? | **Backup** |
| Securely structure production deployment? | **Operational Architecture** |

---

# CSSLP Domain 4 Exam Strategy

Domain 4 questions often ask you to think like an architect rather than a programmer.

The architect should ask:

```text
What are we protecting?
       ↓
Who do we trust?
       ↓
Where does trust change?
       ↓
How does data move?
       ↓
How can the design be attacked?
       ↓
What architectural controls reduce the risk?
```

---

# FIRST Exam Logic

If a question asks what an architect should do **FIRST**, resist jumping immediately to a technology.

Example:

> A company is designing a new payment application. What should the architect do first?

Usually better:

> Understand requirements, assets, trust boundaries, and threats.

Rather than:

- Buy a firewall
- Select an encryption algorithm
- Deploy a WAF
- Purchase a vulnerability scanner

---

# BEST Exam Logic

When two architecture answers appear valid, prefer the answer that:

1. Addresses the risk at the earliest appropriate architectural layer
2. Reduces trust
3. Minimizes attack surface
4. Enforces least privilege
5. Creates clear trust boundaries
6. Avoids single points of failure where availability matters
7. Uses well-understood security mechanisms
8. Supports defense in depth

---

# Architecture Before Implementation

A useful CSSLP principle:

```text
Requirements Problem
→ Fix requirements.

Architecture Problem
→ Fix architecture.

Implementation Problem
→ Fix code.

Deployment Problem
→ Fix deployment.
```

Do not use an implementation control to compensate unnecessarily for a fundamentally insecure architecture.

---

# Example

Problem:

> Every microservice implicitly trusts every other microservice.

Weak response:

> Add additional logging.

Better response:

> Introduce service identity, authentication, authorization, and appropriate trust segmentation.

The better answer addresses the architectural problem itself.

---

# Zero Trust Architecture Reasoning

Even when "Zero Trust" is not explicitly mentioned, remember the principle:

> Do not grant trust solely because of network location.

Instead, evaluate:

- Identity
- Authorization
- Device/workload context
- Resource sensitivity
- Current request

This is particularly useful in:

- Cloud
- Microservices
- Remote access
- Distributed architecture

---

# Blast Radius

Architecture should minimize the damage caused by a single compromise.

Example:

Bad:

```text
One credential
      ↓
Access to every service
      ↓
Access to every database
```

Better:

```text
Service A Credential
→ Service A permissions only

Service B Credential
→ Service B permissions only
```

Think:

> **Compartmentalization + least privilege + limited blast radius**

---

# Single Point of Failure

A single point of failure is a component whose failure causes the entire service to fail.

Example:

```text
Users
  ↓
One Authentication Server
  ↓
Application
```

If that server fails:

> Nobody can authenticate.

Possible improvement:

```text
Authentication Node A
         +
Authentication Node B
```

Think:

> **Redundancy / resilience**

---

# Single Point of Compromise

Different from failure.

Example:

> Every application uses the same administrator credential.

If compromised:

> Every application is exposed.

This is primarily a:

> **Security concentration / blast-radius problem**

---

# Fail Secure

Architecture should define safe behavior when controls fail.

Bad:

```text
Authorization Service Fails
          ↓
Allow Access
```

Better for sensitive access:

```text
Authorization Service Fails
          ↓
Deny Access
```

Think:

> **Fail secure**

---

# Final Exam Rules

### Rule 1

> Architecture should derive from security and business requirements.

### Rule 2

> Identify trust boundaries before selecting controls.

### Rule 3

> Never rely on client-side security enforcement alone.

### Rule 4

> Authentication does not eliminate the need for authorization.

### Rule 5

> Internal network location should not automatically create trust.

### Rule 6

> Minimize attack surface and unnecessary interfaces.

### Rule 7

> Threat modeling should happen before implementation, not only after vulnerabilities appear.

### Rule 8

> STRIDE categorizes threats; PASTA emphasizes business-risk-driven attack analysis.

### Rule 9

> Digital signatures provide firmware/update authenticity and integrity; encryption primarily protects confidentiality.

### Rule 10

> Cloud security follows a shared responsibility model.

### Rule 11

> Backups provide recovery; redundancy and failover provide availability.

### Rule 12

> Architectural reviews should identify systemic security issues early.

### Rule 13

> Security-sensitive management interfaces deserve stronger isolation and controls.

### Rule 14

> Microservices increase the number of interfaces and trust relationships that must be secured.

### Rule 15

> Assess residual risk after architectural controls are applied.

### Rule 16

> If two answers seem correct, prefer the one that fixes the security problem at the architectural layer rather than merely detecting it later.

---

# Domain 4 Master Sequence

Memorize:

```text
Requirements
      ↓
Architecture
      ↓
Assets
      ↓
Trust Boundaries
      ↓
Interfaces
      ↓
Threat Model
      ↓
Attack Surface
      ↓
Controls
      ↓
Risk Review
      ↓
Operational Architecture
```

---

## Disclaimer

This is an independent CSSLP study guide and is not an official ISC2 publication or a collection of official exam questions.
