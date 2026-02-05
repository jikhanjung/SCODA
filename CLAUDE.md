# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

SCODA (Self-Contained Data Artifact) is a conceptual framework and specification for treating data as portable, verifiable, self-describing units of knowledge rather than as services behind APIs. This is a **documentation-only repository** at the concept/specification stage with no runtime implementation.

> "SCODA is Docker for data — but with meaning and memory."

## Repository Structure

- `SCODA_overview.md` - Conceptual foundation and core philosophy (Korean/English)
- `SCODA_v0.1_spec.md` - Minimal technical specification
- `Trilobase_as_SCODA.md` - Real-world example applying SCODA to trilobite taxonomy data
- `README_SCODA.md` - Extended overview with motivation and intended audience

## Core Concepts

**Three Core Principles:**
1. Data is the primary asset — software is only a means
2. The distribution unit is a file, not a service
3. Change happens through versioning, not synchronization

**Five Required Components (SCODA-Core):**
1. **Identity** - artifact_id, name, version, schema_version
2. **Data Payload** - actual content stored internally (no external dependencies)
3. **Semantics** - structural schema, field descriptions, usage guidance
4. **Provenance** - source references, build process, creation timestamp
5. **Integrity** - content hash (SHA-256), versioning rules, optional signature

**Compliance Levels:**
- SCODA-Core: All required components
- SCODA-Extended: Includes optional components (UI, API definitions, saved queries, change logs, annotations)
- SCODA-Derived: Generated from another SCODA artifact

**Explicitly Out of Scope:**
- Real-time synchronization
- Automatic merge semantics
- Multi-user write coordination
- Centralized server dependencies

## Working with This Repository

This repository contains only markdown specifications. There are no build commands, tests, or linting. Contributions involve refining the conceptual framework and specification documents.

When editing specifications, maintain the clear separation between:
- What SCODA **is** (artifact-first, offline-friendly, versioned, immutable)
- What SCODA **is not** (sync system, editing platform, consensus protocol)
