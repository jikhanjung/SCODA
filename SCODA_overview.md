# SCODA — Self-Contained Data Artifact

## Overview

**SCODA (Self-Contained Data Artifact)**는 데이터 자체를 중심에 두고,
그 데이터가 **자기 설명성, 무결성, 버전성**을 갖춘 채
하나의 배포 단위로 존재하도록 설계된 개념입니다.

SCODA는 서비스나 중앙 서버가 아니라,
**파일(artifact)**을 지식과 데이터의 기본 단위로 취급합니다.

---

## One-Sentence Definition

> A **SCODA** is a portable, verifiable unit of data that encapsulates  
> its content, semantics, and identity within a single distributable artifact.

---

## Core Philosophy

1. **Data is the primary asset** — software is only a means.
2. **The distribution unit is a file, not a service.**
3. **Change happens through versioning, not synchronization.**

SCODA는 “항상 최신 상태”를 목표로 하지 않습니다.  
대신 **“의미 있는 시점의 지식 상태”**를 안정적으로 보존하는 것을 목표로 합니다.

---

## What a SCODA Must Contain (Minimal Core)

### 1. Data
- 실제로 의미 있는 콘텐츠
- 데이터베이스, 파일 묶음, 테이블 등 형식은 자유
- SCODA의 중심

### 2. Identity
- 이름
- 고유 식별자
- 버전

→ 이 artifact가 *무엇이며*, *어느 상태*인지를 명확히 식별 가능해야 함

### 3. Semantics
- 스키마
- 필드/엔티티 의미
- 최소한의 사용 설명

→ 데이터는 단순한 값의 집합이 아니라 **이해 가능한 지식**이어야 함

### 4. Integrity
- 출처(provenance)
- 해시 또는 서명
- 변경 시 새 버전이 생성된다는 규칙

→ 신뢰 가능성과 재현성의 기반

---

## What SCODA Explicitly Avoids

다음 요소들은 **핵심 정의에서 의도적으로 제외**됩니다.

- 실시간 동기화
- 자동 병합
- 다중 사용자 편집
- 중앙 서버 의존
- 복잡한 권한/역할 모델

이 문제들은 SCODA의 범위를 넘어서는 **다른 계층의 문제**로 취급합니다.

---

## Optional (Non-Essential) Components

다음 요소들은 SCODA의 본질은 아니지만, 유용할 수 있습니다.

- UI assets (HTML / JS / CSS)
- API 정의
- 표준 질의(saved queries)
- 변경 이력(log)
- 로컬 주석(annotation, notes)

이들은 SCODA의 정체성을 해치지 않는 선에서 선택적으로 포함될 수 있습니다.

---

## Relationship to Existing Concepts

### SQLite Single-File Applications
- SCODA의 훌륭한 구현 수단 중 하나
- 하지만 SQLite 자체가 SCODA의 정의는 아님

### Docker / OCI
- Docker: 실행 환경을 고정하는 artifact
- SCODA: 지식과 데이터 상태를 고정하는 artifact

→ 개념적으로 유사하지만, 목적과 대상이 다름

---

## SCODA and Taxonomic Data (Example Context)

분류학 데이터의 경우:

- Base artifact: 읽기 전용, 권위 있는 스냅샷
- 업그레이드: 새 SCODA 버전 배포
- 개인 해석/메모: 로컬 overlay로 분리

이 구조는 서로 다른 해석(sensu)을 **충돌 없이 공존**하게 하며,
학문적 책임과 재현성을 유지합니다.

---

## Key Takeaway

> **SCODA is not about keeping data synchronized.  
> It is about preserving knowledge as a stable, interpretable artifact.**

---

## Terminology

- **SCODA**: Self-Contained Data Artifact
- **Artifact**: 배포·인용·보존 가능한 단일 파일 단위
- **Base Artifact**: 권위 있고 불변한 기준 데이터
- **Overlay / Annotation**: 로컬에서 추가되는 개인적 정보

---

## Status

This document describes the conceptual foundation of **SCODA**.  
Specification details, runtime behavior, and concrete implementations
are intentionally left out and addressed in separate documents.
