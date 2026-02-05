# SCODA — Self-Contained Data Artifact

SCODA는 **데이터를 하나의 완결된 지식 아티팩트로 취급하기 위한 개념적 프레임워크**입니다.

SCODA에서 데이터는  
서버 뒤에 숨은 자원이 아니라,  
**열어서 이해하고, 인용하고, 보존할 수 있는 단일 배포 단위**입니다.

---

## What is SCODA?

**SCODA (Self-Contained Data Artifact)** is a portable, verifiable unit of data that encapsulates:

- its **content**
- its **meaning**
- its **identity**
- its **provenance**

within a single distributable artifact.

> In short:  
> **SCODA is Docker for data — but with meaning and memory.**

---

## Core Idea

Most modern data systems assume that:

- data lives behind services  
- databases are continuously mutable  
- synchronization is the default  

SCODA challenges this assumption.

Instead, SCODA treats data as:

- a **primary artifact**
- a **snapshot of knowledge**
- a **unit of citation**
- a **self-describing object**

Change happens through **versioned releases**, not silent mutation.

---

## What SCODA Is (and Is Not)

### SCODA **is**
- Artifact-first, not service-first
- Offline-friendly
- Versioned and immutable by default
- Suitable for scholarly and knowledge-centric data
- Independent of any specific database or format

### SCODA **is not**
- A real-time synchronization system
- A collaborative editing platform
- A distributed consensus protocol
- A replacement for databases or APIs

SCODA is a **complementary abstraction**, not a universal solution.

---

## Why SCODA?

SCODA addresses problems that traditional approaches struggle with:

- Long-term reproducibility
- Clear provenance and responsibility
- Preservation of historical knowledge states
- Multiple, conflicting interpretations of the same data
- Use by automated agents (e.g. LLMs) without loss of meaning

These issues are especially prominent in domains such as:  
taxonomy, paleontology, digital humanities, and scientific curation.

---

## Repository Structure

This repository contains **conceptual and specification documents** for SCODA.

```
.
├── README.md
├── SCODA_overview.md        # Conceptual overview and core philosophy
├── SCODA_v0.1_spec.md       # Minimal technical specification
├── Trilobase_as_SCODA.md    # Example: applying SCODA to a real project
├── Why_SCODA.md             # Motivation and position paper
└── docs/                    # (future) additional notes and extensions
```

---

## Status

SCODA is currently at the **concept and specification stage**.

- No reference runtime is defined yet
- No file format is mandated
- Implementations are intentionally left open

The goal of this repository is to:

> **establish a clear conceptual contract before building tools.**

---

## Intended Audience

SCODA is intended for:

- Researchers and curators
- Developers working with scholarly or analytical data
- Anyone dissatisfied with “database-as-a-service” as the only model
- Projects that need durable, citable, interpretable data artifacts

---

## Guiding Principle

> **Some data is better treated as a knowledge artifact  
> than as a continuously mutable service.**

SCODA exists for that kind of data.

---

## License

This repository documents a conceptual framework.  
Unless otherwise noted, all texts are provided for open discussion and reuse.
