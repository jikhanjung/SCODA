# SCODA and Knowledge Objects

## Purpose of This Document

This document clarifies the relationship between **SCODA (Self-Contained Data Artifact)**
and the notion of a **knowledge object**.

When describing SCODA to audiences across different disciplines,
the phrase *knowledge object* frequently arises as an intuitive characterization
of what a SCODA artifact represents.
However, the term carries different meanings in different fields,
and its uncritical use risks introducing unintended connotations.

This document is intended as an explanatory note rather than a foundational definition.
It establishes how the term *knowledge object* should—and should not—be interpreted
in the context of SCODA, and provides usage guidelines for consistency
across SCODA specifications and publications.

For the core definition and governance model of SCODA,
see `SCODA_v0.1_spec.md` and `Why_SCODA.md`.

---

## What Is a "Knowledge Object"?

The term *knowledge object* does not refer to a single, standardized concept.
It appears across multiple fields as a loose descriptor for self-contained,
reusable units of knowledge.

In educational technology, Merrill (2000) introduced *knowledge objects*
as reusable instructional components—discrete containers of knowledge
designed for modular assembly and retrieval.
Wiley (2000) extended this line of thinking through *learning objects*,
emphasizing reusability and contextual adaptability.
In knowledge management, Boisot (1998) described *knowledge assets*
as transferable units of organizational knowledge
that gain value through codification and abstraction.
In digital libraries and scholarly communication,
the term is used informally to denote citable,
self-contained units of research output.

Despite these varied origins, a common intuition persists:

> A *knowledge object* is a discrete unit of knowledge
> that carries meaning, context, and provenance,
> and can be stored, referenced, and reused as a whole.

Importantly, this usage does **not** imply:
- object-oriented programming semantics,
- ontology or knowledge-graph nodes,
- automated reasoning or inference systems.

The term is best understood as a **conceptual shorthand**, not a technical specification.

---

## Common but Unintended Connotations

Because of the word *object*, the phrase *knowledge object* may unintentionally suggest:

- runtime or class-based objects (OOP),
- semantic web entities (RDF, OWL, knowledge graphs),
- AI-interpretable or machine-reasoned units of knowledge.

These interpretations are **explicitly out of scope** for SCODA.
SCODA concerns data *release and governance*, not computational reasoning.
For the full list of what SCODA excludes, see `SCODA_v0.1_spec.md`, "Explicitly Out of Scope."

---

## Why the Term Appears in SCODA Discussions

Although SCODA does not adopt *knowledge object* as a core concept,
the term emerges naturally when explaining what SCODA enables.

A SCODA artifact is self-contained, versioned, immutable by default,
citable as a scholarly unit, and bound to explicit provenance
(see `SCODA_v0.1_spec.md` for the full component specification).
These properties closely match what many communities intuitively mean
when they refer to a *knowledge object*.

In this sense, SCODA does not redefine the term,
but rather **provides concrete conditions under which a dataset
can function as a knowledge object**.

---

## SCODA's Primary Conceptual Commitment

SCODA's central concern is **not** what to call the resulting artifact,
but **how data should behave over time**.

SCODA asserts that:

- meaningful changes produce new artifacts,
- historical states remain accessible and citable,
- interpretive responsibility is attached to specific releases,
- data are treated as released knowledge snapshots, not mutable resources.

This represents a governance choice rather than a serialization choice
(see `Why_SCODA.md`, "SCODA as a Governance Model, Not a Packaging Format").
Whether one chooses to describe such artifacts as *knowledge objects*
is secondary to these commitments.

---

## Recommended Usage in SCODA Documents

To avoid unnecessary connotations, the following guidelines are recommended:

- Use **SCODA** and **self-contained data artifact** as primary terms.
- Introduce *knowledge object* only as an explanatory phrase.
- When used, explicitly narrow its meaning, for example:

> "Here, the term *knowledge object* refers to a released, citable unit of scholarly knowledge,
> without implying object-oriented or ontological semantics."

This keeps the discussion accessible while preventing misinterpretation.

---

## Relationship Summary

The relationship between SCODA and knowledge objects can be summarized as follows:

> **SCODA defines the minimal conditions under which a dataset
> can be treated as a durable knowledge object.**

The emphasis remains on governance, versioning, and responsibility,
not on terminology.

---

## Closing Note

SCODA deliberately avoids introducing new foundational jargon.
Its goal is to clarify practice, not to rename familiar concepts.
The occasional use of *knowledge object* serves only to situate SCODA
within a broader discourse on scholarly data,
and should be read in that limited, descriptive sense.

What matters is not whether the result is called a *knowledge object*,
but whether the data can be understood, verified, and reused without a server—and
whether responsibility for its content is clearly assigned.

---

## References

Boisot, M. (1998). *Knowledge Assets: Securing Competitive Advantage in the Information Economy*. Oxford University Press.

Merrill, M. D. (2000). Knowledge objects and mental models. In D. A. Wiley (Ed.), *The Instructional Use of Learning Objects*. Association for Instructional Technology.

Wiley, D. A. (2000). Connecting learning objects to instructional design theory: A definition, a metaphor, and a taxonomy. In D. A. Wiley (Ed.), *The Instructional Use of Learning Objects*. Association for Instructional Technology.

---

## Status

This document is an explanatory note within the SCODA project.
It clarifies terminology choices and is intended to support
consistent usage across SCODA specifications and publications.
Its core argument may be incorporated into future papers
as a subsection, appendix, or footnote.
