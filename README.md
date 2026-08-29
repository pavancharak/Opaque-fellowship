# TRACE Execution Authorization

## AgentTrust Fellowship 2026 — Technical Proposal

**Pavan Charak · Parmana Systems**

> **Specification & Formal Verification for Authorization-Bound Execution Evidence**

---

## Overview

TRACE provides a foundation for capturing and verifying evidence about AI-agent execution.

TRACE v0.2 already establishes important primitives including policy binding, signed Trust Records, tool-call evidence, delegation, and verification.

This proposal focuses on a narrower standards question:

> **How can an independent verifier determine that an execution matched exactly what was authorized?**

The proposed contribution is an **AuthorizationRecord** that creates a normative, cryptographically verifiable binding between authorization and execution evidence.

The goal is not to replace TRACE's existing attestation model.

The goal is to extend it with a precise **execution-authorization boundary**.

---

## The Standards Gap

Execution evidence answers questions such as:

* What happened?
* Which policy was involved?
* Which runtime executed?
* Which tools were invoked?
* What evidence was recorded?
* Can the evidence be independently verified?

Authorization introduces another question:

> **Was this specific execution actually authorized?**

For regulated and high-consequence environments, that distinction matters.

A system may have valid credentials, valid identity, and valid access while still performing an action that was not authorized under the governing policy.

The proposed AuthorizationRecord makes that relationship explicit.

---

# Proposed Contribution

The fellowship work is intentionally focused on three deliverables:

### 1. AuthorizationRecord Specification

Define a normative authorization object containing:

```text
authorizationId
policyDigest
contentHash
issuedAt
expiresAt
nonce
signature
signatureAlgorithm
```

The specification will define:

* Schema and field semantics
* Authorization lifecycle
* Cryptographic binding
* Expiration semantics
* Replay semantics
* Integration with TRACE processing
* Backward compatibility
* Normative verification behavior
* Accept/reject examples

---

### 2. Formal Security Properties

The authorization model will be expressed as a machine-checkable state machine, with TLA+ as the proposed formalism.

Core properties:

| Property                | Guarantee                                                         |
| ----------------------- | ----------------------------------------------------------------- |
| No-replay               | A consumed authorization cannot be successfully reused            |
| No-tamper               | Authorized content cannot be substituted with different content   |
| No-expiry-bypass        | Expired authorization cannot successfully verify                  |
| Deterministic-rejection | Verification failures have explicit outcomes                      |
| Signature-binding       | An authorization cannot be verified under a different signing key |

The objective is not merely to provide tests.

The objective is to establish security properties that can be independently examined and formally checked.

---

### 3. Reference Implementation

A TypeScript/Python reference implementation will demonstrate:

```text
ISSUE
  ↓
AuthorizationRecord
  ↓
VERIFY
  ├── Signature
  ├── Policy digest
  ├── Content hash
  ├── Time validity
  └── Nonce state
  ↓
CONSUME
  ↓
EXECUTE / REJECT
  ↓
TRACE EVIDENCE
```

The reference implementation will provide normative examples and conformance evidence for independent implementers.

---

# TRACE + Parmana

The proposal builds directly on the overlap between TRACE and Parmana.

### TRACE provides the evidence layer

TRACE already provides mechanisms around:

* Trust Records
* Policy binding
* Execution evidence
* Tool transcripts
* Delegation
* Signatures
* Verification
* Conformance

### Parmana provides execution-authority experience

Parmana has been developed around:

* Policy-driven execution
* Deterministic verification
* Fail-closed execution
* Signed execution attestations
* Replay protection
* Content binding
* Named rejection outcomes
* Independent cryptographic verification

### The proposed bridge

The contribution is therefore:

```text
              TRACE
        Execution Evidence
                 │
                 │
                 ▼
       ┌───────────────────┐
       │ AuthorizationRecord│
       │                   │
       │ What was allowed? │
       │ Under which policy│
       │ For what content? │
       │ For how long?     │
       └─────────┬─────────┘
                 │
                 ▼
              PARMANA
        Execution Authority
                 │
                 ▼
        EXECUTE / REJECT
```

The result is a stronger verification question:

> **Did the observed execution correspond to a valid authorization?**

---

# Six-Month Work Plan

## Months 1–2 — RFC 001

### TRACE Authorization Specification

Deliver:

* AuthorizationRecord schema
* Field semantics
* Cryptographic semantics
* Lifecycle definition
* TRACE integration points
* Backward compatibility
* Five normative examples
* 50+ schema/validation tests

### Target outcome

* RFC accepted through the relevant standards process
* Two or more independent implementations
* Interoperable generation and verification

---

## Months 2–4 — RFC 002

### Formal Verification & Security Properties

Deliver:

* Formal authorization model
* TLA+ state machine
* Five core security properties
* Property-based test suite
* Named rejection semantics
* Independent conformance testing
* Peer review

### Target outcome

* Machine-checkable model
* 50+ property tests
* Published conformance evidence
* Independent implementations producing consistent outcomes

---

## Months 4–6 — Reference Implementation + TRACE v0.3

Deliver:

* TypeScript/Python reference implementation
* AuthorizationRecord generation
* Signature verification
* Content-hash verification
* TTL validation
* Nonce consumption
* Named rejection outcomes
* TRACE integration
* Normative examples
* Live reference implementation

### Target outcome

A working reference showing:

```text
Authorization
      ↓
Cryptographic verification
      ↓
Execution
      ↓
TRACE evidence
      ↓
Independent verification
```

---

# What Is Intentionally Out of Scope

This proposal deliberately does **not** attempt to solve every adjacent problem.

Not required for the core contribution:

* Full AGT policy → authorization mapping
* Confidential MCP architecture
* Policy-to-authorization CLI tooling
* Broad productization
* Commercial deployment architecture
* Blog/conference program

These areas can be discussed with the TRACE/AgentTrust community where they materially affect interoperability or the standards roadmap.

**Implementation priorities will be finalized collaboratively after discussion with the TRACE/AgentTrust community.**

---

# Why This Contribution Matters

The central standards question is simple:

> **Can an independent party prove that an agent's execution was authorized?**

A useful answer requires three layers:

### 1. Evidence

What actually happened?

### 2. Authorization

What was permitted to happen?

### 3. Verification

Can an independent party mathematically and deterministically establish that the two correspond?

This proposal focuses specifically on the second and third layers while building on TRACE's existing evidence infrastructure.

---

# Parmana as Implementation Evidence

Parmana already implements an execution-authority architecture involving:

* Signed authorization records
* Ed25519 + ML-DSA-65
* Independent verification
* Content binding
* Replay protection
* Expiry enforcement
* Fail-closed execution
* Named security outcomes

The current Parmana proposal reports **208 passing tests** across its authorization and security-property coverage.

These results are implementation evidence.

They are **not being presented as TRACE conformance** until independently evaluated against the TRACE specification.

That distinction is intentional.

The fellowship would turn implementation experience into a standards contribution.

---

# Success Metrics

| Period    | Deliverable                           | Success                                   |
| --------- | ------------------------------------- | ----------------------------------------- |
| Month 1–2 | RFC 001                               | Accepted + 2 implementations              |
| Month 2–4 | RFC 002                               | Peer-reviewed + 50+ tests                 |
| Month 4–6 | TRACE v0.3 + reference implementation | Published + live + independently verified |

---

# Public Technical Basis

The proposal is based on the publicly available TRACE specification and roadmap.

* **TRACE v0.2:** Trust Record, policy binding, execution evidence, delegation, signatures and verification
* **TRACE roadmap:** planned evolution toward future TRACE versions and profiles
* **Action receipts:** public TRACE materials identify receipt-oriented extensions as an area for future development

The proposed AuthorizationRecord is intended to complement these capabilities rather than duplicate them.

---

# Live Reference

### Parmana

https://parmana.fly.dev

### GitHub

https://github.com/pavancharak

---

# Core Thesis

TRACE can establish trustworthy evidence of execution.

The next standards question is whether that evidence can be bound to **authority** with equal precision.

This proposal contributes:

**AuthorizationRecord**

*

**Formal Security Properties**

*

**Reference Implementation**

=

**Independently Verifiable Execution Authorization**

---

## Conclusion

TRACE already provides a strong foundation for trustworthy AI-agent execution evidence.

This fellowship proposes a focused extension:

**Specify what authorization means.**

**Formally verify its security properties.**

**Demonstrate interoperable verification.**

The result is a standards-level mechanism for answering:

> **Did the executor do exactly what was authorized?**

**Ready to build standards with you.**

---

**Pavan Charak**
Parmana Systems
AgentTrust Fellowship 2026
