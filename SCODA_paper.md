# SCODA: Self-Contained Data Artifacts for Versioned, Citable Knowledge Snapshots

**Author:** Jikhan Jung  
**Affiliation:** (to be added)  
**Corresponding author:** (to be added)

---

## Keywords
Self-contained data artifact; data governance; scholarly databases; taxonomy; versioned data; reproducibility; provenance; data citation

---

## Abstract
We propose **SCODA**—**Se**lf-**Co**ntained **Da**ta artifact—as a governance-oriented model for distributing scholarly and knowledge-centric datasets as **versioned, immutable, and citable artifacts**. SCODA is motivated by a recurring mismatch between service-centered database practices optimized for continuously mutable “latest” states and scholarly requirements for reproducibility, accountability, and long-term interpretability. In domains such as taxonomy—where multiple interpretations (*sensu*) may legitimately coexist—SCODA treats a dataset release as a durable knowledge snapshot rather than a perpetually synchronized service. We position SCODA not as a competing data packaging format, but as a normative contract that can incorporate existing standards (e.g., Frictionless Data, RO-Crate, BagIt) while enforcing versioned identity, provenance, semantics, and integrity as first-class requirements.

---

## 1. Introduction
Digital research infrastructures increasingly rely on centralized databases and web services to store, update, and disseminate data. This paradigm is well suited for transactional and operational systems, where the primary concern is maintaining a consistent and up-to-date state. However, scholarly data practices place different demands on data systems. Researchers must be able to cite exact data states, reproduce analyses years later, and interpret datasets correctly without relying on the continued availability of a specific service or software environment.

In practice, these requirements are often addressed through ad hoc solutions such as database dumps, supplementary files, or informal archival conventions. Such approaches, while useful, lack a clear conceptual framework that defines what a “released dataset” is and how it should behave over time. This paper introduces **SCODA (Self-Contained Data Artifact)** as a minimal yet explicit model for addressing this gap.

The core premise of SCODA is simple: a dataset does not need to be continuously updated to remain valuable. Instead, it must clearly state **which knowledge state** it represents. By shifting the default assumption from mutable services to immutable releases, SCODA aims to align data infrastructure with the epistemic needs of scholarly work.

---

## 2. Motivation: The Limits of Service-Centered Data Models

### 2.1 Service Bias and the “Latest State” Assumption
Modern data systems implicitly prioritize the notion of a single, authoritative “current” state. While historical changes may be logged internally, user-facing interfaces typically present only the latest version of the data. This design choice simplifies interaction but introduces several problems in scholarly contexts.

First, **reproducibility** is compromised when analyses depend on data that may silently change. Second, **accountability** becomes diffuse, as responsibility for interpretive decisions is spread across a sequence of incremental updates rather than attached to discrete, citable releases. Third, **interpretability** degrades when data are separated from the assumptions and sources that motivated their structure.

### 2.2 Taxonomy as a Motivating Example
Taxonomic data provide a particularly clear illustration of these issues. In taxonomy, disagreement and reinterpretation are not exceptional but expected. A taxon name may be circumscribed differently depending on the author, year, or evidentiary basis—a distinction commonly expressed through the *sensu* convention.

From a data modeling perspective, such differences can be understood as alternative interpretations applied to shared underlying facts. Informally, a *sensu* corresponds to a view over a common factual substrate. Crucially, these views are not merely convenience constructs; they are scholarly claims tied to specific sources and must therefore be preserved, cited, and compared.

Service-centered models tend to obscure this plurality by privileging a single canonical view. SCODA instead treats interpretive diversity as a first-class property by anchoring interpretations to versioned artifacts.

---

## 3. Why Not Just Add Metadata?

A common objection is that existing data packaging standards already support rich metadata and could therefore express snapshot semantics through additional fields. While technically correct, this argument overlooks the distinction between *expressibility* and *enforcement*.

In many existing ecosystems, versioning and provenance metadata are optional. Workflows and tools continue to assume in-place updates and preferential access to the latest state. As a result, snapshot semantics remain advisory rather than foundational.

SCODA differs by making immutability and versioned identity mandatory. A meaningful change results in a new artifact, and historical states remain accessible and citable by design. This shift represents a change in governance rather than serialization: it defines how data are allowed to evolve and how responsibility is assigned.

---

## 4. Definition of a Self-Contained Data Artifact

A **SCODA** is defined as a distributable artifact that encapsulates the following components:

1. **Data** – the dataset itself, in any suitable storage format (e.g., SQLite, Parquet).
2. **Identity** – a unique artifact identifier, version, and schema version.
3. **Semantics** – sufficient structural and interpretive information to support correct reuse.
4. **Provenance** – explicit references to sources, methods, and build processes.
5. **Integrity** – cryptographic hashes and, optionally, digital signatures.

SCODA intentionally excludes real-time synchronization, automatic conflict resolution, and collaborative editing from its core definition. These functions may be provided by external systems but are not required for an artifact to qualify as a SCODA.

---

## 5. SCODA and Canonical Servers

SCODA does not replace servers. Instead, it clarifies their role. Servers remain the most effective means of providing interactive access, search, visualization, and APIs. However, servers emphasize availability and currency, whereas SCODA emphasizes durability and citability.

We therefore propose a layered model:

1. **Canonical Server** – provides the latest recommended interpretation and interactive access.
2. **SCODA Releases** – provide immutable, versioned snapshots of knowledge states.
3. **Local Overlays** – allow users to maintain personal annotations or alternative interpretations without mutating released artifacts.

This separation of concerns reduces the burden on servers to function as archival systems and enables clearer attribution of responsibility to specific releases.

---

## 6. Related Work

Several existing efforts address aspects of data packaging and preservation. **Frictionless Data** focuses on lightweight data exchange with explicit schemas. **RO-Crate** emphasizes research context and provenance through structured metadata. **BagIt** provides mechanisms for integrity checking and archival transfer.

These approaches solve important problems and can be incorporated within SCODA artifacts. However, none of them explicitly define a governance model centered on immutable, versioned knowledge snapshots. SCODA builds on these foundations while addressing a complementary set of concerns.

---

## 7. Discussion and Scope

SCODA is not intended as a universal solution for all data management problems. It is most applicable in domains where data represent authoritative or interpretive knowledge states and where citation, reproducibility, and accountability are primary concerns. Examples include taxonomy, digital humanities, legal corpora, and curated reference datasets.

Future work includes the development of reference tooling for creating, validating, and distributing SCODA artifacts, as well as empirical evaluation of the model in operational systems.

---

## 8. Conclusion

SCODA reframes data distribution from continuous service access to the release of durable knowledge artifacts. By enforcing immutability, versioned identity, and explicit provenance, SCODA aligns data infrastructure with scholarly practices and accommodates interpretive plurality. Rather than competing with existing standards, SCODA provides a governance layer that clarifies how data should evolve, be cited, and be trusted over time.

---

## References

Adrain, J. M. (2011). Class Trilobita Walch, 1771. In Z.-Q. Zhang (Ed.), *Animal biodiversity: An outline of higher-level classification and survey of taxonomic richness*. Zootaxa, 3148, 104–109.

Jell, P. A., & Adrain, J. M. (2002). Available generic names for trilobites. *Memoirs of the Queensland Museum*, 48(2), 331–553.

Boettiger, C., et al. (2015). A framework for reproducible and reusable research data. *Scientific Data*, 2, 150040.

RO-Crate Community. (2023). *RO-Crate Metadata Specification*.  

Frictionless Data. (2023). *Data Package Specification*.  

BagIt Working Group. (2019). *BagIt File Packaging Format*.
