# Why SCODA?
## Why We Need Self-Contained Data Artifacts

---

## The Problem This Document Addresses

Modern data systems overwhelmingly assume that:

- Data lives behind services
- Software is the primary product
- Databases are continuously mutable and synchronized

This model works well for transactional systems,
but it fails for **scholarly, analytical, and knowledge-centric data**.

SCODA is proposed as a response to this mismatch.

---

## The Limits of Service-Centered Data

### Services Are Ephemeral

Web services change, migrate, or disappear.
Yet scholarly data must remain:

- Citable
- Verifiable
- Interpretable years later

A URL is not a stable scholarly reference.
A versioned artifact can be.

---

### Synchronization Obscures Responsibility

Continuously synchronized databases make it difficult to answer:

- Who changed this?
- When?
- Based on what evidence?

For knowledge data, silent updates undermine trust.

---

### Data Without Semantics Does Not Travel

Raw datasets rarely carry:

- Their conceptual assumptions
- Their interpretive boundaries
- Their intended scholarly use

When data moves without meaning, misuse is inevitable.

---

## Why Existing Approaches Fall Short

### Databases

Databases excel at storage and querying,
but are poorly suited for:

- Long-term preservation
- Citation
- Representing historical knowledge states

---

### APIs

APIs provide access, not ownership.
They require infrastructure to remain available
and cannot be cited as durable scholarly objects.

---

### Data Packaging Standards

Standards such as **Frictionless Data**, **RO-Crate**, and **BagIt**
solve important problems in data exchange, research context,
and archival integrity.

However, they treat data primarily as:

- A transferable resource
- An input to downstream processes
- Something expected to change in place

They do not define how data should behave
as an authoritative knowledge snapshot.

---

## Why Not Just Add Metadata to Existing Standards?

A natural question is whether SCODA could be replaced
by adding a few metadata fields to existing data packaging standards.

After all, these standards already support rich metadata.
Why not simply annotate datasets with versioning or snapshot information?

The answer is that **SCODA is not defined by additional metadata,
but by a different default assumption about what data *is*.**

---

### Metadata as an Option vs. Metadata as a Foundation

Existing standards allow snapshot-related information to be expressed,
but they do not *require* it.

In Frictionless Data, RO-Crate, or BagIt:
- Versioning is optional
- Mutability is assumed
- Updates typically overwrite prior states
- Tools do not treat historical states as first-class entities

Snapshot semantics, if present, are treated as supplementary hints.

In SCODA:
- Versioned identity is mandatory
- Artifacts are immutable by default
- Any meaningful change produces a new artifact
- Historical states are first-class, citable objects

If this information is missing, the artifact is simply **not a SCODA**.

---

### Different Default Behavior

The distinction is not what *can* be expressed,
but what is enforced as the default behavior.

| Question | Existing Standards | SCODA |
|--------|-------------------|-------|
| Is the data expected to change in place? | Yes | No |
| Is “latest” implicitly preferred? | Yes | No |
| Are past states central or peripheral? | Peripheral | Central |
| Is silent mutation acceptable? | Often | Never |
| Is the artifact itself a unit of citation? | Optional | Mandatory |

SCODA explicitly rejects the assumption
that data should always converge toward a single,
continuously updated state.

---

### Responsibility and Accountability

In knowledge-centric domains,
change represents an interpretive decision.

Metadata can describe what happened,
but it cannot prevent silent revision
or ambiguity of authority.

SCODA enforces responsibility by design:

- Authority is attached to a specific artifact version
- Revisions are explicit, not implicit
- Disagreement results in parallel artifacts, not hidden overrides

This is a governance choice, not a serialization choice.

---

### SCODA as a Governance Model, Not a Packaging Format

SCODA does not compete with existing standards.
It can incorporate them.

A SCODA artifact may:
- Contain a Frictionless Data Package as its payload
- Embed RO-Crate metadata for research context
- Use BagIt-style checksums for integrity

What SCODA adds is a **normative contract**:
how data is allowed to evolve, be cited,
and trusted over time.

---

## Why SCODA Is Especially Relevant Now

Several recent shifts make SCODA timely:

- Local analysis tools normalize working with standalone data artifacts
- Versioned data thinking emphasizes historical states
- Supply-chain trust demands provenance and verification
- AI systems increasingly consume data without human intermediaries

In this context, embedded meaning and responsibility become essential.

---

## A Minimal Claim

SCODA does not claim to solve all data problems.

It claims only this:

> **Some data is better treated as a durable knowledge artifact
> than as a continuously mutable service.**

---

## Closing Thought

As computation becomes cheaper and interfaces more automated,
the weakest link in data workflows is no longer access,
but **meaning and trust**.

SCODA addresses this gap by making data:

- Portable
- Interpretable
- Verifiable
- Durable

---

## Status

This document is a **position statement**.
It motivates SCODA as a governance concept,
not as a finalized technical standard.
