# Embedding UI in the Database
## A Draft Design Note for SCODA-Compatible Artifacts

---

## 1. Purpose

This document outlines a **minimal, practical design** for embedding user interface (UI)
definitions directly inside a database file, with particular emphasis on **SCODA-compatible
self-contained data artifacts**.

The goal is not to build a full application framework, but to ensure that:

> A data artifact can be opened, explored, and interpreted **without requiring
> external UI configuration or bespoke software per dataset**.

The central question this document addresses is:

> **"When a user opens this artifact for the first time,
> how should the data be presented?"**

The answer should be embedded in the artifact itself,
so that any compatible viewer can produce a meaningful default view.

---

## 2. Motivation

In many scholarly workflows, data are distributed as files (e.g. SQLite databases),
but meaningful use still depends on:

- prior knowledge of schema,
- custom scripts or applications,
- external documentation.

Embedding UI information in the database addresses this gap by coupling:

- **what the data are**,
- **how the data should be viewed**, and
- **what the creator's primary display intent is**

within the same artifact.

This approach aligns with SCODA's principle that released data represent
**interpretable knowledge snapshots**, not raw resources.

---

## 3. Design Principles

The following principles guide this design:

1. **Self-containment**
   All information required for basic exploration lives inside the artifact.

2. **Separation of concerns**
   The database describes *what to show*; a generic runtime decides *how to render it*.

3. **Declarative first**
   UI behavior is described declaratively where possible, not as executable code.

4. **Graceful degradation**
   The artifact must be interpretable at every level of UI richness.
   If detailed UI assets are absent, the manifest guides rendering.
   If the manifest is absent, display intent provides a reasonable default.
   If even display intent is absent, the schema itself remains readable.

5. **Runtime neutrality**
   No dependency on a specific UI framework (Qt, web, desktop).

6. **Security by default**
   Embedded UI must not implicitly grant arbitrary code execution.

---

## 4. Conceptual Model

The embedded UI is composed of four layers, ordered from most fundamental to most expressive:

```
Level 0: Display Intent
  Semantic hint — "this data is best viewed as a tree"
  Minimal, runtime-independent, always interpretable

Level 1: UI Manifest
  Declarative specification — view definitions, field mappings, layout rules
  Richer than intent, but still runtime-neutral

Level 2: Saved Queries
  Named, parameterized queries — stable data access interface
  Connects UI layers to underlying tables

Level 3: UI Assets
  Literal resources — HTML/CSS/JS for pixel-precise presentation
  Optional progressive enhancement, may depend on specific runtimes
```

Each level builds on the previous one. Higher levels provide richer experiences
but are never required. A single, generic **viewer runtime** is responsible
for loading and rendering whichever layers are present.

### Semantic vs. Declarative vs. Literal

The choice between semantic UI definition (what the data means visually),
declarative UI specification (how views are structured),
and literal UI assets (exact rendering) is not an either-or decision.
It is a question of layers:

| Approach | Portability | Expressiveness | Durability | SCODA Fit |
|----------|------------|----------------|------------|-----------|
| **Semantic** (Display Intent) | Highest | Minimal | Highest | Core philosophy |
| **Declarative** (UI Manifest) | High | Moderate | High | Natural extension |
| **Literal** (UI Assets) | Low | Full | Low | Progressive enhancement |

Literal assets that are absent do not diminish the artifact's interpretability.
Assets provide a *better* experience, not the *only* experience.

---

## 5. Minimal Database Schema

### 5.1 Display Intent

Display intent captures the **creator's primary intention for how each data entity
should be viewed**. It is the most minimal UI layer—sufficient for any viewer
to produce a reasonable default presentation without additional configuration.

```sql
CREATE TABLE ui_display_intent (
  id INTEGER PRIMARY KEY,
  entity TEXT NOT NULL,
  default_view TEXT NOT NULL,
  description TEXT,
  source_query TEXT,
  priority INTEGER DEFAULT 0
);
```

**Field descriptions:**

| Field | Purpose |
|-------|---------|
| `entity` | The data concept this intent describes (e.g., `"genera"`, `"references"`) |
| `default_view` | A semantic view type from the standard vocabulary (see below) |
| `description` | Human-readable explanation of why this view is appropriate |
| `source_query` | Optional reference to a named query in `ui_queries` |
| `priority` | Ordering when multiple intents exist for the same entity (0 = primary) |

#### View Type Vocabulary

The following view types constitute a minimal standard vocabulary.
Implementations may extend this set, but these types should be recognized
by any SCODA-compatible viewer.

| View Type | Meaning | Suitable Data |
|-----------|---------|---------------|
| `tree` | Hierarchical structure navigation | Taxonomic ranks, file structures, organizational hierarchies |
| `table` | Flat listing with search, sort, and filter | Species lists, references, measurements |
| `detail` | Single-record detailed view | Individual taxon pages, paper metadata |
| `map` | Spatial visualization | Collection localities, distribution records |
| `timeline` | Temporal axis visualization | Geological ages, publication years |
| `graph` | Relationship network | Synonym relationships, citation networks |

#### Example: Trilobase

```sql
INSERT INTO ui_display_intent (entity, default_view, description, priority) VALUES
  ('genera',     'tree',  'Taxonomic hierarchy is the primary organizational structure', 0),
  ('genera',     'table', 'Flat listing for search and filtering',                       1),
  ('references', 'table', 'Literature references sorted by year',                        0),
  ('synonyms',   'graph', 'Synonym relationships as a network',                          0);
```

With this information alone, a viewer that has never seen Trilobase before
can present genera as a navigable tree, references as a sortable table,
and synonyms as a relationship graph—without any manifest or embedded assets.

---

### 5.2 UI Manifest

The UI manifest provides **declarative, structured view definitions**
that go beyond display intent. It specifies field mappings, layout rules,
and view-specific configurations.

```sql
CREATE TABLE ui_manifest (
  id INTEGER PRIMARY KEY,
  name TEXT NOT NULL,
  version TEXT NOT NULL,
  manifest_json TEXT NOT NULL,
  created_at TEXT NOT NULL
);
```

The `manifest_json` field contains a declarative specification, for example:

```json
{
  "default_view": "taxonomy_tree",
  "views": {
    "taxonomy_tree": {
      "type": "tree",
      "source": "taxonomic_ranks",
      "label_field": "name",
      "children_field": "parent_id",
      "detail_fields": ["author", "year", "status"]
    },
    "genera_table": {
      "type": "table",
      "source": "genera_list",
      "columns": ["name", "family", "order", "author", "year"],
      "default_sort": "name",
      "searchable": true
    }
  }
}
```

The manifest refines display intent with concrete field-level instructions.
It remains declarative and runtime-neutral.

Manifests are versioned independently via the `version` field.
UI improvements can be released without changing the underlying data.
However, if the data itself changes, a new SCODA artifact must be produced.

---

### 5.3 Saved Queries

Saved queries expose meaningful, named data access patterns.

```sql
CREATE TABLE ui_queries (
  id INTEGER PRIMARY KEY,
  name TEXT NOT NULL,
  description TEXT,
  sql TEXT NOT NULL,
  params_json TEXT,
  created_at TEXT NOT NULL
);
```

These queries act as a **stable interface layer** between the UI and raw tables.
Display intents and manifests may reference queries by name via the `source_query`
or `source` fields, decoupling view definitions from raw schema details.

---

### 5.4 Optional UI Assets

For richer presentation, static UI assets may be embedded.

```sql
CREATE TABLE ui_assets (
  id INTEGER PRIMARY KEY,
  path TEXT NOT NULL UNIQUE,
  mime_type TEXT NOT NULL,
  content BLOB NOT NULL,
  sha256 TEXT NOT NULL,
  created_at TEXT NOT NULL
);
```

UI assets are optional. They provide pixel-level control over presentation
but introduce dependencies on specific rendering technologies (e.g., web browsers).
If absent, the runtime falls back to lower layers.

---

## 6. Runtime Responsibilities

A generic viewer runtime performs the following steps:

1. Open the database artifact (read-only by default)
2. Detect which UI layers are present
3. Render according to the **fallback chain** described below
4. Execute only explicitly declared queries

### Fallback Chain

The runtime selects its rendering strategy based on which layers are available,
preferring the most expressive layer present:

```
1. UI Assets present     → Render using embedded assets
2. UI Manifest present   → Interpret manifest and render declaratively
3. Display Intent present → Generate default views from intent hints
4. None of the above     → Fall back to schema-based generic table view
```

At every level, the artifact remains interpretable.
The fallback chain ensures that **removing a higher layer never breaks the artifact**—
it only reduces visual richness.

### Supported Interactions

For read-only artifacts, the following interactions are appropriate:

- **Navigation**: tree expand/collapse, pagination, breadcrumbs
- **Filtering**: conditional search, column sorting
- **Detail view**: record selection leading to detailed display
- **View switching**: toggling between alternative views of the same entity (e.g., tree ↔ table)

Write operations (editing, deleting, inserting) belong to the **local overlay** layer
and are outside the scope of the embedded UI definition.

The runtime **does not** infer UI behavior from schema alone
when display intent or manifest information is available.

---

## 7. Security Considerations

Embedding UI information introduces potential risks.

Recommended safeguards include:

- read-only database access by default,
- strict content security policy for embedded assets,
- no external network access unless explicitly enabled,
- hash verification for embedded assets (`sha256` field in `ui_assets`),
- clear separation between data and executable logic.

Declarative layers (display intent, manifest, queries) are inherently safer
than literal assets (HTML/CSS/JS). Runtimes should apply stricter sandboxing
to embedded assets than to declarative layers.

---

## 8. Relationship to SCODA

In the SCODA framework:

- the database file is the **data artifact**,
- the embedded UI constitutes part of its **interpretive layer**,
- the viewer runtime is *not* part of the artifact.

Embedding UI information strengthens the artifact's role as a
**self-describing, interpretable knowledge snapshot**.

### Compliance Levels

The current `SCODA_v0.1_spec.md` lists Interface Metadata as an optional component.
The four UI layers defined in this document map to SCODA compliance levels as follows:

| Compliance Level | Display Intent | UI Manifest | Saved Queries | UI Assets |
|-----------------|----------------|-------------|---------------|-----------|
| **SCODA-Core** | Not required | Not required | Not required | Not required |
| **SCODA-Extended** | Strongly recommended (SHOULD) | Optional (MAY) | Optional (MAY) | Optional (MAY) |

- With Display Intent → the artifact is **"open and immediately understandable"**
- Without Display Intent → the artifact is still a valid SCODA, but users must read the schema directly

This preserves the existing spec structure while providing a clear path
for artifacts that aim to be immediately interpretable.

---

## 9. Non-Goals

This design explicitly does **not** aim to:

- replace full-featured applications,
- support collaborative editing,
- embed arbitrary runtime code,
- standardize UI aesthetics,
- define a universal UI description language.

It defines only the minimum required for *portable interpretability*.

---

## 10. Design Decisions

The following questions, originally left open, have been resolved in this revision:

### How expressive should the UI manifest be?

The separation of Display Intent and UI Manifest resolves this question.
Display Intent provides the minimum (view type + entity + description).
The manifest adds as much detail as needed (field mappings, sort rules, filter conditions).
Neither layer is required to carry the full burden of expressiveness.

### Should manifests be versioned independently?

**Yes.** The `ui_manifest.version` field supports independent versioning.
UI improvements can be delivered without changing the underlying data.
However, changes to the data payload require a new SCODA artifact,
as mandated by the SCODA immutability principle.

### How should multiple manifests coexist?

Two mechanisms are available:
1. **Priority ordering** via the `priority` field in `ui_display_intent`,
   which determines the default view when multiple options exist.
2. **Audience targeting** (future extension): an `audience` field
   (e.g., `"researcher"`, `"student"`, `"general"`) could allow
   different default views for different user groups.

### What level of interactivity is appropriate?

For read-only artifacts, appropriate interactions include
navigation, filtering, sorting, detail views, and view switching.
Write operations belong to the local overlay layer,
not to the embedded UI definition.

---

## 11. Summary

Embedding UI definitions inside a database enables:

- single-file distribution,
- immediate interpretability,
- domain-specific presentation,
- alignment with SCODA's artifact-first philosophy.

The four-layer model ensures that interpretability degrades gracefully:

> **Display Intent** guarantees a reasonable default view.
> **UI Manifest** refines it with declarative structure.
> **Saved Queries** provide stable data access.
> **UI Assets** add visual polish when a capable runtime is available.

At every level, the artifact remains openable, understandable, and usable.

---

## Status

This document is a **design draft** within the SCODA project.
It proposes a layered approach to embedding UI definitions in data artifacts
and is intended as a basis for discussion, prototyping, and eventual
integration into the SCODA specification.
