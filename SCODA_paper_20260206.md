# SCODA: Self-Contained Data Artifacts for Versioned, Citable Knowledge Snapshots

**Author:** Jikhan Jung
**Affiliation:** (to be added)
**Corresponding author:** (to be added)

---

## Keywords
Self-contained data artifact; data governance; scholarly databases; taxonomy; versioned data; reproducibility; provenance; data citation; knowledge object; display intent

---

## Abstract
We propose **SCODA**—**Se**lf-**Co**ntained **Da**ta artifact—as a governance-oriented model for distributing scholarly and knowledge-centric datasets as **versioned, immutable, and citable artifacts**. SCODA is motivated by a recurring mismatch between service-centered database practices optimized for continuously mutable "latest" states and scholarly requirements for reproducibility, accountability, and long-term interpretability. In domains such as taxonomy—where multiple interpretations (*sensu*) may legitimately coexist—SCODA treats a dataset release as a durable knowledge snapshot rather than a perpetually synchronized service. We further introduce a layered model for embedded interpretability, in which artifacts declare *display intent*—semantic hints about how data should be presented—enabling immediate exploration without dataset-specific tooling. We illustrate the model through Trilobase, a trilobite genus-level taxonomic database distributed as a SCODA artifact.

---

## 1. Introduction
Digital research infrastructures increasingly rely on centralized databases and web services to store, update, and disseminate data. This paradigm is well suited for transactional and operational systems, where the primary concern is maintaining a consistent and up-to-date state. However, scholarly data practices place different demands on data systems. Researchers must be able to cite exact data states, reproduce analyses years later, and interpret datasets correctly without relying on the continued availability of a specific service or software environment.

In practice, these requirements are often addressed through ad hoc solutions such as database dumps, supplementary files, or informal archival conventions. Such approaches, while useful, lack a clear conceptual framework that defines what a "released dataset" is and how it should behave over time. This paper introduces **SCODA (Self-Contained Data Artifact)** as a minimal yet explicit model for addressing this gap.

The core premise of SCODA is simple: a dataset does not need to be continuously updated to remain valuable. Instead, it must clearly state **which knowledge state** it represents. By shifting the default assumption from mutable services to immutable releases, SCODA aims to align data infrastructure with the epistemic needs of scholarly work.

---

## 2. Motivation: The Limits of Service-Centered Data Models

### 2.1 Service Bias and the "Latest State" Assumption
Modern data systems implicitly prioritize the notion of a single, authoritative "current" state. While historical changes may be logged internally, user-facing interfaces typically present only the latest version of the data. This design choice simplifies interaction but introduces several problems in scholarly contexts.

First, **reproducibility** is compromised when analyses depend on data that may silently change. Second, **accountability** becomes diffuse, as responsibility for interpretive decisions is spread across a sequence of incremental updates rather than attached to discrete, citable releases. Third, **interpretability** degrades when data are separated from the assumptions and sources that motivated their structure.

### 2.2 Taxonomy as a Motivating Example
Taxonomic data provide a particularly clear illustration of these issues. In taxonomy, disagreement and reinterpretation are not exceptional but expected. A taxon name may be circumscribed differently depending on the author, year, or evidentiary basis—a distinction commonly expressed through the *sensu* convention.

From a data modeling perspective, such differences can be understood as alternative interpretations applied to shared underlying facts. Informally, a *sensu* corresponds to a view over a common factual substrate. Crucially, these views are not merely convenience constructs; they are scholarly claims tied to specific sources and must therefore be preserved, cited, and compared.

Service-centered models tend to obscure this plurality by privileging a single canonical view. SCODA instead treats interpretive diversity as a first-class property by anchoring interpretations to versioned artifacts.

---

## 3. Why Not Just Add Metadata?

A common objection is that existing data packaging standards already support rich metadata and could therefore express snapshot semantics through additional fields. While technically correct, this argument overlooks the distinction between *expressibility* and *enforcement*.

Consider three widely adopted standards:

- **Frictionless Data** (Open Knowledge Foundation, 2023) defines a `datapackage.json` descriptor that can include a `version` field and source references. However, versioning is optional and there is no mechanism to prevent in-place updates to the underlying CSV files. A Frictionless Data Package with a `version` field is not thereby immutable; it simply carries a label that may or may not reflect its actual change history.

- **RO-Crate** (Soiland-Reyes et al., 2022) wraps research artifacts in a `ro-crate-metadata.json` file conforming to Schema.org. It can express rich provenance, authorship, and temporal information. Yet RO-Crate is designed primarily for describing research context, not for enforcing release governance. A crate may be updated in place, and tools do not distinguish between a "released snapshot" and a "working draft" at the structural level.

- **BagIt** (Kunze et al., 2018) provides manifest-based checksums that verify the integrity of a payload after transfer. It ensures that data have not been corrupted in transit, but it does not define versioning semantics, provenance requirements, or the expectation that a bag, once published, should not be silently replaced.

In each case, versioning and snapshot semantics *can* be expressed, but they are not *enforced*. Workflows and tools continue to assume in-place updates and preferential access to the latest state. As a result, snapshot semantics remain advisory rather than foundational.

The following table summarizes the difference in default assumptions:

| Question | Existing Standards | SCODA |
|--------|-------------------|-------|
| Is the data expected to change in place? | Yes | No |
| Is "latest" implicitly preferred? | Yes | No |
| Are past states central or peripheral? | Peripheral | Central |
| Is silent mutation acceptable? | Often | Never |
| Is the artifact itself a unit of citation? | Optional | Mandatory |

SCODA differs by making immutability and versioned identity mandatory. A meaningful change results in a new artifact, and historical states remain accessible and citable by design. This shift represents a change in governance rather than serialization: it defines how data are allowed to evolve and how responsibility is assigned.

---

## 4. Definition of a Self-Contained Data Artifact

### 4.1 Required Components

A **SCODA** is defined as a distributable artifact that encapsulates the following components:

1. **Data** – the dataset itself, in any suitable storage format (e.g., SQLite, Parquet).
2. **Identity** – a unique artifact identifier, version, and schema version.
3. **Semantics** – sufficient structural and interpretive information to support correct reuse.
4. **Provenance** – explicit references to sources, methods, and build processes.
5. **Integrity** – cryptographic hashes and, optionally, digital signatures.

SCODA intentionally excludes real-time synchronization, automatic conflict resolution, and collaborative editing from its core definition. These functions may be provided by external systems but are not required for an artifact to qualify as a SCODA.

### 4.2 SCODA and the Notion of "Knowledge Object"

The properties listed above—self-containment, versioned identity, citability, embedded semantics, and explicit provenance—closely match what many communities intuitively describe as a *knowledge object*: a discrete unit of knowledge that carries meaning, context, and provenance, and can be stored, referenced, and reused as a whole.

The term *knowledge object* has appeared in several disciplines. In educational technology, Merrill (2000) introduced *knowledge objects* as reusable instructional components, and Wiley (2000) extended this through *learning objects*. In knowledge management, Boisot (1998) described *knowledge assets* as transferable units of organizational knowledge. In scholarly communication, the term is used informally to denote citable, self-contained units of research output.

SCODA does not adopt *knowledge object* as a defining term, nor does it claim to redefine it. Rather, SCODA provides **concrete, enforceable conditions** under which a dataset can function as a durable knowledge object. The emphasis remains on governance—how data are allowed to evolve, be cited, and be trusted—rather than on terminology. In this paper, the occasional use of the phrase *knowledge object* refers to a released, citable unit of scholarly knowledge, without implying object-oriented or ontological semantics.

### 4.3 Embedded Interpretability: Display Intent

The five required components described in Section 4.1 ensure that an artifact is identifiable, verifiable, and traceable. However, when a user opens an unfamiliar SCODA artifact for the first time, a question remains: **how should the data be presented?**

Without guidance, users must inspect raw schemas and tables to understand what they are looking at. This imposes a significant barrier to immediate interpretability—one of SCODA's stated goals. To address this, SCODA introduces the concept of **display intent** as an optional but strongly recommended component for SCODA-Extended artifacts.

Display intent is a minimal, semantic declaration of **how the artifact's creator intended the data to be viewed**. It does not prescribe pixel-level rendering or depend on any specific UI framework. Instead, it provides hints that any compatible viewer can use to produce a reasonable default presentation.

A display intent declaration associates each data entity with a **view type** drawn from a standard vocabulary:

| View Type | Meaning |
|-----------|---------|
| `tree` | Hierarchical structure navigation |
| `table` | Flat listing with search, sort, and filter |
| `detail` | Single-record detailed view |
| `map` | Spatial visualization |
| `timeline` | Temporal axis visualization |
| `graph` | Relationship network |

For example, a taxonomic dataset might declare that genera should be viewed primarily as a tree (reflecting taxonomic hierarchy) and secondarily as a table (for search and filtering), while literature references should be viewed as a sortable table.

Display intent occupies the most fundamental level of a four-layer model for embedded interpretability:

1. **Display Intent** (semantic) – minimal view type hints; runtime-independent
2. **UI Manifest** (declarative) – structured view definitions with field mappings and layout rules
3. **Saved Queries** (data access) – named, parameterized queries providing a stable interface layer
4. **UI Assets** (literal) – embedded HTML/CSS/JS for pixel-precise presentation

Each layer builds on the previous one, and higher layers are never required. This design follows a principle of **graceful degradation**: if UI assets are absent, the manifest guides rendering; if the manifest is absent, display intent provides a reasonable default; if even display intent is absent, the schema itself remains readable. At every level, the artifact remains interpretable.

This layered approach resolves the tension between semantic and literal UI definitions. Semantic declarations (display intent) maximize portability and durability. Literal assets (HTML/CSS/JS) maximize expressiveness but introduce framework dependencies. By treating these as complementary layers rather than competing alternatives, SCODA allows artifacts to be immediately understandable in any viewer while optionally supporting richer presentation in capable runtimes.

---

## 5. SCODA and Canonical Servers

SCODA does not replace servers. Instead, it clarifies their role. Servers remain the most effective means of providing interactive access, search, visualization, and APIs. However, servers emphasize availability and currency, whereas SCODA emphasizes durability and citability.

We therefore propose a layered model (Figure 1):

1. **Canonical Server** – provides the latest recommended interpretation and interactive access.
2. **SCODA Releases** – provide immutable, versioned snapshots of knowledge states.
3. **Local Overlays** – allow users to maintain personal annotations or alternative interpretations without mutating released artifacts.

```
┌─────────────────────────────────────────────────────────┐
│                   Canonical Server                      │
│          (latest view, search, API access)              │
│                                                         │
│   - Continuously updated                                │
│   - Interactive exploration                             │
│   - Recommended interpretation                          │
└────────────────────────┬────────────────────────────────┘
                         │ releases
                         ▼
┌─────────────────────────────────────────────────────────┐
│                    SCODA Release                        │
│       (versioned, immutable, citable snapshot)          │
│                                                         │
│   - Read-only after publication                         │
│   - Bound to specific provenance                        │
│   - Usable without server access                        │
└────────────────────────┬────────────────────────────────┘
                         │ extends
                         ▼
┌─────────────────────────────────────────────────────────┐
│                    Local Overlay                        │
│     (personal annotations, alternative sensu views)     │
│                                                         │
│   - Does not modify released artifact                   │
│   - Not synchronized across users                       │
│   - Supports independent scholarly reasoning            │
└─────────────────────────────────────────────────────────┘

Figure 1. The SCODA layered model. The canonical server provides
the latest view; SCODA releases preserve citable knowledge states;
local overlays support personal interpretation without mutation.
```

This separation of concerns reduces the burden on servers to function as archival systems and enables clearer attribution of responsibility to specific releases.

---

## 6. Case Study: Trilobase

To illustrate how SCODA operates in practice, we describe **Trilobase**, a genus-level taxonomic database for trilobites (Arthropoda: Trilobita), structured and distributed as a SCODA artifact.

### 6.1 Project Overview

Trilobase compiles genus-level taxonomic information for trilobites, drawing primarily on Jell & Adrain (2002) and Adrain (2011). It records genus names, higher-rank classifications, synonymy relationships, and associated literature references. The dataset is distributed as a single SQLite file containing all taxonomic tables, metadata, and provenance information.

### 6.2 Mapping to SCODA Components

| SCODA Component | Trilobase Implementation |
|----------------|--------------------------|
| **Data** | SQLite database containing taxonomic tables (genera, families, orders, synonyms, references) |
| **Identity** | Project name (`trilobase`) + semantic version (e.g., `v1.0.0`) |
| **Semantics** | Database schema with rank hierarchy, synonym relation types, and field descriptions |
| **Provenance** | Source literature citations (Jell & Adrain 2002; Adrain 2011), build timestamp, curator identity |
| **Integrity** | SHA-256 hash of the released artifact; immutable release policy |

### 6.3 Versioning and Immutability

Each released version of Trilobase is treated as read-only. When taxonomic revisions require changes—such as the addition of newly described genera or the reclassification of existing ones—a new version of the artifact is produced. The previous version remains available and citable, preserving the exact knowledge state that prior analyses were based on.

This mirrors the epistemic practice of taxonomy itself: taxonomic opinions evolve through publication, not through silent mutation of prior records.

### 6.4 Handling Multiple Interpretations (*Sensu*)

Taxonomic disagreement is an expected condition, not an error state. Trilobase accommodates this by allowing multiple taxonomic concepts to coexist within the dataset. Each concept is explicitly labeled with its authority and year (e.g., *sensu* Adrain, 2011), and each assertion is traceable to a specific literature reference.

The default distributed artifact may present one recommended concept set, while preserving alternatives. Users may select, compare, or extend these views through local overlays without modifying the released artifact.

### 6.5 Local Extension

When a researcher opens Trilobase locally, the base artifact remains immutable. Local modifications are limited to personal overlays: notes, annotations, alternative interpretations, and links to additional literature. These extensions do not overwrite canonical taxonomy and are not automatically synchronized across installations. This separation ensures that the authoritative record and personal scholarly reasoning remain clearly distinguishable.

### 6.6 Embedded Display Intent

Trilobase embeds display intent declarations that enable any SCODA-compatible viewer to present the data meaningfully without prior knowledge of the dataset's structure. The following intents are included in the artifact:

| Entity | View Type | Rationale |
|--------|-----------|-----------|
| Genera | `tree` (primary) | Taxonomic hierarchy is the principal organizational structure |
| Genera | `table` (secondary) | Flat listing supports search and filtering across genera |
| References | `table` | Literature references are best explored as a sortable, searchable list |
| Synonyms | `graph` | Synonym relationships form a network that is most naturally visualized as a graph |

A viewer encountering Trilobase for the first time can use these declarations to immediately present genera as a navigable tree, references as a sortable table, and synonym relationships as a network graph—without requiring any external documentation or dataset-specific configuration.

This demonstrates how display intent, even without a full UI manifest or embedded assets, fulfills SCODA's goal of producing artifacts that are not merely verifiable and citable, but also **immediately interpretable**.

---

## 7. Related Work

Several existing efforts address aspects of data packaging, preservation, and citation. SCODA builds on these foundations while addressing a complementary set of concerns centered on governance and immutability.

### 7.1 Data Packaging Standards

**Frictionless Data** (Open Knowledge Foundation, 2023) focuses on lightweight data exchange with explicit schemas, using JSON-based descriptors to define tabular data packages. **RO-Crate** (Soiland-Reyes et al., 2022) emphasizes research context and provenance through structured metadata conforming to Schema.org. **BagIt** (Kunze et al., 2018) provides mechanisms for integrity checking and archival transfer through manifest-based checksums.

These approaches solve important problems in data exchange, research documentation, and archival integrity. A SCODA artifact may incorporate any of these standards—for example, using a Frictionless Data Package as its payload or BagIt-style checksums for integrity verification. However, none of these standards explicitly define a governance model centered on immutable, versioned knowledge snapshots. Versioning is optional, mutability is assumed, and tools do not treat historical states as first-class entities.

### 7.2 FAIR Principles

The **FAIR Principles** (Wilkinson et al., 2016) assert that data should be Findable, Accessible, Interoperable, and Reusable. SCODA is broadly compatible with FAIR but differs in emphasis. FAIR focuses on maximizing the *discoverability and reusability* of data regardless of its governance model. SCODA focuses on ensuring that a specific *knowledge state* remains citable, verifiable, and interpretable over time. A SCODA artifact can be FAIR-compliant, but being FAIR-compliant does not require the immutability and versioned identity that SCODA enforces.

### 7.3 Data Citation Principles

The **FORCE11 Joint Declaration of Data Citation Principles** (Data Citation Synthesis Group, 2014) establishes that data citations should be accorded the same importance as citations of other research objects and should include persistent identifiers, versioning, and provenance. SCODA operationalizes these principles by defining a concrete artifact structure in which versioned identity, provenance, and integrity are mandatory rather than aspirational.

### 7.4 Versioned Data in Data Engineering

In data engineering, **time travel** capabilities in systems such as Delta Lake (Armbrust et al., 2020) and Apache Iceberg allow users to query historical states of datasets. These systems demonstrate the practical value of versioned data access. However, they operate within centralized, service-dependent architectures and are designed for large-scale analytical workloads rather than scholarly citation. SCODA addresses a different use case: producing portable, self-contained artifacts that can be cited and verified without server access.

---

## 8. Discussion and Scope

SCODA is not intended as a universal solution for all data management problems. It is most applicable in domains where data represent authoritative or interpretive knowledge states and where citation, reproducibility, and accountability are primary concerns. Examples include taxonomy, digital humanities, legal corpora, and curated reference datasets.

The Trilobase case study demonstrates that the SCODA model maps naturally onto existing scholarly data practices, particularly in domains where interpretive plurality is expected and where the distinction between authoritative records and personal reasoning must be preserved.

SCODA does not prescribe a specific file format, runtime environment, or distribution mechanism. This flexibility is intentional: it allows the governance model to be applied across diverse technical contexts. However, reference tooling for creating, validating, and distributing SCODA artifacts would lower adoption barriers and is a priority for future work.

The display intent mechanism introduced in Section 4.3 and demonstrated in Section 6.6 addresses a practical gap that governance alone cannot fill. An artifact that is verifiable and citable but opaque to casual inspection still imposes a significant barrier to adoption. By embedding minimal, semantic hints about how data should be viewed, SCODA artifacts can be immediately interpretable without requiring dataset-specific viewers or external documentation. The four-layer model (display intent, manifest, queries, assets) ensures that this interpretability degrades gracefully across viewers of varying capability, from simple schema browsers to full-featured rendering environments.

Additional directions for future work include:
- Formal validation criteria for SCODA compliance levels (Core, Extended, Derived)
- Standardization of display intent vocabulary and manifest schema
- Integration patterns with existing data packaging standards (e.g., embedding SCODA metadata within RO-Crate or Frictionless Data packages)
- Development of a reference SCODA viewer that implements the four-layer fallback chain
- Empirical evaluation of the model across multiple scholarly domains

---

## 9. Conclusion

SCODA reframes data distribution from continuous service access to the release of durable knowledge artifacts. By enforcing immutability, versioned identity, and explicit provenance, SCODA aligns data infrastructure with scholarly practices and accommodates interpretive plurality. The embedded display intent mechanism further ensures that artifacts are not only verifiable and citable, but also immediately interpretable—bridging the gap between governance and usability. The Trilobase case study illustrates how these principles apply concretely to taxonomic data, where multiple interpretations coexist and where the integrity of historical knowledge states is essential.

Rather than competing with existing standards, SCODA provides a governance layer that clarifies how data should evolve, be cited, and be trusted over time. What matters is not whether the result is called a knowledge object, a data package, or a dataset release, but whether it can be understood, verified, and reused without a server—and whether responsibility for its content is clearly assigned.

---

## References

Adrain, J. M. (2011). Class Trilobita Walch, 1771. In Z.-Q. Zhang (Ed.), *Animal biodiversity: An outline of higher-level classification and survey of taxonomic richness*. Zootaxa, 3148, 104–109.

Armbrust, M., Das, T., Sun, L., Yavuz, B., Zhu, S., Murthy, M., Torres, J., van Hovell, H., Ionescu, A., Łuszczak, A., Switakowski, M., Sasaki, M., Li, X., Luo, S., Nham, V., & Zaharia, M. (2020). Delta Lake: High-performance ACID table storage over cloud object stores. *Proceedings of the VLDB Endowment*, 13(12), 3411–3424. https://doi.org/10.14778/3415478.3415560

Boisot, M. (1998). *Knowledge Assets: Securing Competitive Advantage in the Information Economy*. Oxford University Press.

Data Citation Synthesis Group. (2014). Joint Declaration of Data Citation Principles. FORCE11. https://doi.org/10.25490/a97f-egyk

Jell, P. A., & Adrain, J. M. (2002). Available generic names for trilobites. *Memoirs of the Queensland Museum*, 48(2), 331–553.

Kunze, J., Littman, J., Madden, L., Scancella, J., & Adams, C. (2018). The BagIt File Packaging Format (V1.0). RFC 8493. https://doi.org/10.17487/RFC8493

Merrill, M. D. (2000). Knowledge objects and mental models. In D. A. Wiley (Ed.), *The Instructional Use of Learning Objects*. Association for Instructional Technology.

Open Knowledge Foundation. (2023). *Frictionless Data: Data Package Specification*. https://specs.frictionlessdata.io

Soiland-Reyes, S., Sefton, P., Crosas, M., Castro, L. J., Coppens, F., Fernández, J. M., Garber, D., Groth, P., La Rosa, M., Leo, S., Ó Carragáin, E., Rabadan, M., Shanahan, H., Strber, D., Thelen, J., & Grüning, B. (2022). Packaging research artefacts with RO-Crate. *Data Science*, 5(2), 97–138. https://doi.org/10.3233/DS-210053

Wiley, D. A. (2000). Connecting learning objects to instructional design theory: A definition, a metaphor, and a taxonomy. In D. A. Wiley (Ed.), *The Instructional Use of Learning Objects*. Association for Instructional Technology.

Wilkinson, M. D., Dumontier, M., Aalbersberg, I. J., Appleton, G., Axton, M., Baak, A., ... & Mons, B. (2016). The FAIR Guiding Principles for scientific data management and stewardship. *Scientific Data*, 3, 160018. https://doi.org/10.1038/sdata.2016.18
