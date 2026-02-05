# SCODA v0.1 Specification
## Minimal Technical Specification for Self-Contained Data Artifacts

---

## Purpose

This document defines **SCODA v0.1**, a *minimal, implementation-agnostic specification*
for creating and distributing **Self-Contained Data Artifacts**.

SCODA v0.1 intentionally focuses on the **smallest viable standard**
required to ensure portability, interpretability, and trust.

---

## Design Principles

1. **Artifact-first**: The artifact is the primary unit of distribution.
2. **Implementation-neutral**: No storage format or runtime is mandated.
3. **Immutable-by-default**: Published artifacts are treated as read-only.
4. **Versioned evolution**: Change occurs by producing a new artifact.
5. **Offline-first**: No network dependency is assumed.

---

## Required Components

A SCODA v0.1 compliant artifact **MUST** provide the following components.

---

### 1. Identity

The artifact must be uniquely identifiable.

**Required fields**
- `artifact_id` (globally unique identifier)
- `name`
- `version`
- `schema_version`

**Properties**
- `artifact_id` must remain stable for the lifetime of the artifact version
- Any content change requires a new `version`

---

### 2. Data Payload

The artifact must contain the actual data it represents.

**Requirements**
- Data must be stored internally within the artifact
- External data dependencies are not allowed
- Data format is unrestricted (e.g., relational tables, columnar files)

---

### 3. Semantics

The artifact must describe how its data should be interpreted.

**Required elements**
- Structural schema
- Entity and field descriptions
- Minimal usage guidance

**Goal**
- Enable correct interpretation without external documentation

---

### 4. Provenance

The artifact must document its origin.

**Required elements**
- Source references (datasets, literature, upstream artifacts)
- Build process or tool identifier
- Creation timestamp

**Purpose**
- Support reproducibility and scholarly accountability

---

### 5. Integrity

The artifact must provide a mechanism to verify its integrity.

**Required elements**
- Content hash (e.g., SHA-256)
- Clear versioning rule

**Optional**
- Digital signature
- Public key reference

---

## Optional Components

The following components are allowed but not required by SCODA v0.1.

---

### Interface Metadata

- UI entrypoints
- API definitions
- CLI commands

Purpose: enable immediate usability without external tooling.

---

### Query Definitions

- Saved queries
- Named views
- Parameterized templates

Purpose: provide stable, interpretable access patterns.

---

### Change Log

- Append-only record of changes
- Domain-level change descriptions

Purpose: capture evolution without modifying the base artifact.

---

### Annotations / Overlays

- Personal notes
- Alternative interpretations
- Non-authoritative extensions

Purpose: allow local reasoning without compromising canonical data.

---

## Explicitly Out of Scope

SCODA v0.1 does **not** define:

- Real-time synchronization
- Distributed consensus
- Automatic merge semantics
- Multi-user write coordination
- Authorization or identity systems

These concerns belong to higher-level systems.

---

## Compliance Levels

- **SCODA-Core**: Implements all required components
- **SCODA-Extended**: Includes optional components
- **SCODA-Derived**: Generated from another SCODA artifact

---

## Versioning Rules

- Patch-level changes require a new artifact version
- Major semantic changes require a new `schema_version`
- Backward compatibility is recommended but not required

---

## Summary

SCODA v0.1 defines the **minimum contract**
that transforms a data file into a durable, interpretable knowledge artifact.

> **If an artifact can be understood, verified, and reused without a server,
> it qualifies as a SCODA.**

---

## Status

This specification is **experimental** and intended as a foundation
for discussion, refinement, and practical validation.
