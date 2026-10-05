# CSSLP Domain 1 Study Guide  
## Secure Software Concepts

This study guide is designed to help recognize common **CSSLP Domain 1** concepts quickly, especially in scenario-based and **BEST / MOST appropriate** exam questions.

---

## Core Security Principles and Exam Triggers

| Trigger / Keyword | Principle / Concept | What It Means | Common Exam Trap |
|---|---|---|---|
| Too many permissions | **Least Privilege** | Give only the minimum access needed | Confusing it with segregation of duties |
| One person controls the entire process | **Segregation of Duties** | Split sensitive tasks among different people | Picking least privilege because both reduce risk |
| Access checked only once | **Complete Mediation** | Recheck authorization when protected resources are accessed | Choosing authentication instead of authorization |
| Shared memory / shared temp area / shared account | **Least Common Mechanism** | Minimize unnecessary shared mechanisms or resources | Confusing shared resources with component reuse |
| Secret algorithm / hidden design | **Open Design** | Security should not depend on secrecy of the design | Thinking secret design automatically means stronger security |
| Security mechanism is overly complex | **Economy of Mechanism** | Keep security mechanisms simple and understandable | Assuming centralization is always secure |
| Strong control used alone | **Defense in Depth** | Use multiple independent layers of protection | Treating one strong control as sufficient |
| Security failure automatically allows access | **Fail Secure** | Fail into a safe state, usually denying access | Prioritizing availability over security |
| Users bypass inconvenient controls | **Psychological Acceptability** | Security should be usable enough that users follow it | Assuming stricter controls always improve security |
| Need to know exactly who performed an action | **Accountability** | Actions must be attributable to a specific identity | Confusing accountability with nonrepudiation |
| A person should not be able to deny an action | **Nonrepudiation** | Provide strong evidence linking an action to an actor | Thinking immutable logs alone guarantee it |
| MFA proves identity | **Authentication** | Verify who the user is | Confusing identity verification with permissions |
| Transaction denied because role lacks permission | **Authorization** | Decide what an authenticated user can do | Picking authentication because login was mentioned |
| Hash detects modification | **Integrity** | Protect against unauthorized change | Assuming hashing proves who created the data |
| Digital signature | **Integrity + Authenticity + Nonrepudiation Support** | Verifies origin and detects modification | Confusing signing with encryption |
| Reusing a vetted security module | **Component Reuse** | Prefer proven components over custom reinvention | Confusing reuse with shared runtime resources |
| Internal standard conflicts with law | **Governance / Compliance** | Applicable law or regulation takes precedence | Assuming stricter internal policy always wins |

---

# High-Value Exam Distinctions

## Least Privilege vs. Segregation of Duties

### Least Privilege

Ask:

> **How much access does this person, process, or system receive?**

A subject should receive only the permissions required to perform its function.

### Segregation of Duties

Ask:

> **How many people must participate in the sensitive process?**

Critical actions are divided among multiple people so that one individual cannot control the entire process.

### Example

A developer can create a production release but cannot approve or deploy it.

- Restricted permissions → **Least Privilege**
- Different people must approve and deploy → **Segregation of Duties**

---

## Authentication vs. Authorization

### Authentication

Authentication answers:

> **Who are you?**

Examples:

- Password
- MFA
- Smart card
- Certificate
- Biometric verification

### Authorization

Authorization answers:

> **What are you allowed to do?**

Examples:

- Read a file
- Approve a transaction
- Access an API endpoint
- Modify a database table

### Exam Shortcut

```text
Authentication = Identity
Authorization  = Permission
```

---

## Accountability vs. Nonrepudiation

### Accountability

Accountability answers:

> **Who performed the action?**

Common controls include:

- Unique user accounts
- Audit logs
- Session tracking
- Administrative activity logging

### Nonrepudiation

Nonrepudiation answers:

> **Can the person credibly deny performing the action?**

Common supporting technologies include:

- Digital signatures
- Trusted timestamps
- Strong identity binding
- Protected audit evidence

### Exam Trap

An immutable log does **not automatically provide nonrepudiation**.

If five administrators use one shared account, the system may prove:

```text
The administrator account performed the action.
```

But it cannot reliably prove:

```text
Alice performed the action.
```

---

## Economy of Mechanism vs. Least Common Mechanism

These two are easy to confuse.

### Economy of Mechanism

Think:

> **Keep it simple.**

Security mechanisms should be small, simple, understandable, and easy to review.

Example:

Instead of 40 applications implementing their own authentication systems, use a small, centrally maintained authentication mechanism.

### Least Common Mechanism

Think:

> **Share as little as possible.**

Minimize mechanisms and resources shared between users, applications, or security domains.

Examples:

- Shared memory
- Shared temporary directories
- Shared privileged accounts
- Shared security-sensitive caches

### Quick Comparison

```text
Economy of Mechanism     = Minimize complexity
Least Common Mechanism   = Minimize sharing
```

---

## Integrity vs. Nonrepudiation

### Integrity

Integrity answers:

> **Was the information changed?**

Common controls:

- Cryptographic hashes
- MACs
- Digital signatures
- File integrity monitoring

### Nonrepudiation

Nonrepudiation focuses on:

> **Who performed the action, and can they credibly deny it?**

### Example

A SHA-256 hash may show that a software file changed.

It does **not**, by itself, prove who created or published the software.

---

# Security Design Principles

## Least Privilege

Give users, applications, and processes only the permissions necessary to perform their required functions.

### Exam Clues

Look for:

- Excessive permissions
- Administrator access when read-only is sufficient
- Broad database privileges
- Overprivileged service accounts

### Memory Phrase

> **Only what you need.**

---

## Segregation of Duties

Divide critical functions between multiple individuals.

This helps reduce:

- Fraud
- Abuse
- Accidental mistakes
- Unauthorized changes

### Exam Clues

Look for:

- One person creating and approving transactions
- Developers deploying their own production code
- One administrator controlling an entire sensitive workflow

### Memory Phrase

> **No one person controls everything.**

---

## Complete Mediation

Every access to a protected resource should be checked against the current authorization policy.

### Problem Example

A user receives administrator access.

Later, the administrator role is revoked.

However, the application continues trusting permissions cached when the session began.

This violates **complete mediation**.

### Exam Clues

Look for:

- Authorization checked only at login
- Cached permissions
- Stale sessions
- Revoked privileges remaining effective

### Memory Phrase

> **Check every access.**

---

## Least Common Mechanism

Minimize shared mechanisms between different users, applications, or security domains.

### Problem Example

Several applications store sensitive information in the same shared memory segment.

A vulnerability in one application allows it to read data belonging to the others.

### Exam Clues

Look for:

- Shared memory
- Shared directories
- Shared accounts
- Shared security-sensitive resources

### Memory Phrase

> **Share as little as possible.**

---

## Open Design

Security should not depend on keeping the system's design secret.

A system should remain secure even if attackers understand how it works.

### Example

Bad design:

```text
Our encryption algorithm is secure because nobody knows how it works.
```

Better design:

```text
The algorithm can be publicly known.
Security depends on protecting the cryptographic key.
```

### Memory Phrase

> **The design can be public.**

---

## Economy of Mechanism

Security mechanisms should be as simple and small as practical.

Complex systems are:

- Harder to understand
- Harder to test
- Harder to audit
- More likely to contain vulnerabilities

### Exam Clues

Look for:

- Many duplicated security mechanisms
- Overly complicated access-control systems
- Excessive custom security logic

### Memory Phrase

> **Keep security simple.**

---

## Defense in Depth

Use multiple independent layers of security.

Do not depend entirely on one control.

Example:

```text
MFA
+
Authorization
+
Network Segmentation
+
Logging
+
Monitoring
```

If one control fails, the others can still provide protection.

### Memory Phrase

> **Use multiple layers.**

---

## Fail Secure

When a security mechanism fails, the system should remain in a secure state.

Example:

If an authorization server becomes unavailable:

Bad:

```text
Authorization unavailable → Allow access
```

Better:

```text
Authorization unavailable → Deny privileged access
```

### Exam Phrase

This is often described as:

```text
Fail Closed
```

rather than:

```text
Fail Open
```

### Memory Phrase

> **Failure should not grant access.**

---

## Psychological Acceptability

Security controls should be practical and usable.

If security is too difficult, users may create unsafe workarounds.

### Example

A company requires:

- 24-character passwords
- Multiple complexity rules
- Password changes every 14 days

Employees respond by writing passwords on sticky notes.

The technical control may appear strong, but poor usability creates new security risks.

### Memory Phrase

> **Usable security gets followed.**

---

# Component Reuse

When possible, use proven and well-tested security components rather than implementing security-sensitive functions repeatedly.

Examples include:

- Authentication libraries
- Cryptographic libraries
- Key-management systems
- Input-validation libraries
- Secure session-management components

### Exam Scenario

Five development teams independently implement cryptographic key storage.

Each implementation contains different vulnerabilities.

A better approach is to use:

> **One vetted and centrally maintained security component.**

---

# Digital Signatures, Hashing, and Encryption

These technologies are frequently mixed together in exam questions.

## Hashing

Primarily supports:

> **Integrity**

A hash can help determine whether data has changed.

Example:

```text
SHA-256(file) → digest
```

However, if an attacker can replace both:

```text
file
+
hash
```

the user may still accept the malicious file.

---

## Digital Signatures

Digital signatures support:

- Integrity
- Authenticity
- Nonrepudiation evidence

Conceptually:

```text
Hash the data
      ↓
Sign the hash using the private key
      ↓
Verify using the public key
```

If the signature verifies, the recipient has stronger assurance about both the origin and integrity of the data.

---

## Encryption

Encryption primarily supports:

> **Confidentiality**

It answers:

> Can unauthorized people read the information?

### Exam Shortcut

```text
Encryption         → Confidentiality
Hashing            → Integrity
Digital Signature  → Integrity + Authenticity + Nonrepudiation support
```

---

# Fail Open vs. Fail Closed

## Fail Open

A failure causes access to be granted.

```text
Authorization service unavailable
        ↓
Allow access
```

This usually increases security risk.

---

## Fail Closed

A failure causes access to be denied.

```text
Authorization service unavailable
        ↓
Deny access
```

For security-sensitive functions, this commonly represents **fail secure** behavior.

---

# Immutable vs. Read-Only

These terms are related but are not identical.

## Immutable

Immutable means:

> **Existing information cannot be changed after it has been recorded.**

Many immutable systems are effectively:

```text
Append-only
```

Existing records cannot be modified, but new records can still be added.

---

## Read-Only

Read-only generally means:

> **No new information can be written.**

### Comparison

```text
Immutable / Append-only:
Old records → cannot change
New records → can be added

Read-only:
Old records → cannot change
New records → cannot be added
```

In security questions, immutable logs primarily help protect:

> **Integrity**

---

# Fast Exam Recognition Table

| If You See... | Think... |
|---|---|
| Excessive permissions | **Least Privilege** |
| One person controls everything | **Segregation of Duties** |
| Authorization checked only once | **Complete Mediation** |
| Shared resource exposes other users/apps | **Least Common Mechanism** |
| Security depends on secret design | **Open Design** |
| Too much security complexity | **Economy of Mechanism** |
| One control protecting everything | **Defense in Depth** |
| Failure grants access | **Fail Secure problem** |
| Users bypass difficult security controls | **Psychological Acceptability** |
| Need to identify who did something | **Accountability** |
| User should not be able to deny an action | **Nonrepudiation** |
| Verify identity | **Authentication** |
| Determine allowed actions | **Authorization** |
| Detect unauthorized modification | **Integrity** |
| Proven reusable security functionality | **Component Reuse** |

---

# Fast Memory Phrases

```text
Least Privilege
→ Only what you need.

Segregation of Duties
→ No one person controls everything.

Complete Mediation
→ Check every access.

Least Common Mechanism
→ Share as little as possible.

Open Design
→ The design can be public.

Economy of Mechanism
→ Keep security simple.

Defense in Depth
→ Use multiple layers.

Fail Secure
→ Failure should not grant access.

Psychological Acceptability
→ Usable security gets followed.

Accountability
→ Know who did it.

Nonrepudiation
→ They cannot credibly deny it.

Authentication
→ Who are you?

Authorization
→ What can you do?

Integrity
→ Was it changed?
```

---

# CSSLP Exam Strategy

CSSLP questions frequently contain more than one technically reasonable answer.

When the question asks for:

- **BEST**
- **MOST appropriate**
- **PRIMARY**
- **MOST directly**

focus on the specific weakness described in the scenario.

For example:

A user's permissions are revoked, but the application continues accepting previously cached authorization.

Several principles may be relevant, but the most direct problem is:

> **Complete Mediation**

The application is failing to reevaluate authorization when protected resources are accessed.

## Final Rule

When two answers seem correct, ask:

> **Which principle most directly addresses the problem described in the scenario?**

That is often the answer ISC2-style questions are looking for.

---

## Disclaimer

This is an independent study guide for CSSLP preparation and is not an official ISC2 publication or set of official exam questions.
