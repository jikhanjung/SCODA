# SCODA_UI_Embedded_DB_Draft.md Review

SCODA_UI_Embedded_DB_Draft.md에 대한 평가 및 개선 제안 정리

---

## 1. 강점

### 3계층 구조가 명확함
UI Manifest / Saved Queries / UI Assets로 구성된 모델이 깔끔하다. 각 계층의 역할이 분명하고, SQL 스키마 예시가 구체적이어서 구현 방향을 바로 잡을 수 있다.

### 핵심 분리 원칙이 정확함
> "The database describes *what to show*; a generic runtime decides *how to render it*."

이 한 문장이 설계의 핵심을 잘 요약한다. Artifact와 runtime의 책임 경계가 명확하다.

### 경계 설정이 잘 되어 있음
Security Considerations, Non-Goals 섹션에서 "full application 대체가 아니다", "임의 코드 실행 금지" 등을 명시한 것이 좋다. SCODA의 read-only 철학과 일관된다.

### SCODA와의 관계 정립이 적절함
Section 8에서 "viewer runtime은 artifact의 일부가 아니다"라고 명시한 것이 중요하다. UI 정보는 artifact에 속하지만, 렌더링 엔진은 외부 도구이다.

---

## 2. 핵심 문제: Display Intent 계층이 빠져 있음

### 2.1 문제 진단

현재 문서는 UI manifest의 구조를 정의하고 있지만, 그보다 더 근본적인 질문에 답하지 않는다:

> **"이 데이터를 처음 열었을 때, 어떻게 보여야 하는가?"**

manifest_json 예시에 `"type": "tree"` 같은 정보가 포함되어 있지만, 이것이:
- 데이터의 **의미적 성격에 대한 힌트**인지 (semantic),
- **구체적인 UI 렌더링 지시**인지 (declarative),

구분이 없다. 이 둘은 목적과 추상화 수준이 다르므로 분리해야 한다.

### 2.2 제안: Display Intent를 Level 0으로 추가

기존 3계층 아래에 **Display Intent**를 최하위 계층으로 추가하여 4계층 구조로 확장한다:

```
Level 0: Display Intent (최소, semantic)
  → "이 데이터는 트리로 보는 것이 1차적 의도이다"
  → 어떤 런타임이든 reasonable한 기본 뷰를 만들 수 있는 힌트
  → manifest나 assets가 없어도 독립적으로 작동

Level 1: UI Manifest (선언적, declarative)
  → 구체적인 뷰 정의, 필드 매핑, 레이아웃 구조
  → 런타임이 해석하여 렌더링
  → Display Intent를 구체화

Level 2: Saved Queries (데이터 접근 계층)
  → 이름 있는 파라미터 쿼리
  → UI와 raw 테이블 사이의 안정적 인터페이스

Level 3: UI Assets (리터럴, optional)
  → HTML/CSS/JS로 정확한 표현을 지정
  → 특정 런타임에 의존, progressive enhancement
```

### 2.3 Display Intent가 중요한 이유

Display Intent만 있는 경우에도:
- 뷰어가 "genera는 트리로, references는 테이블로 보여주면 되겠구나" 판단 가능
- manifest나 assets 없이도 artifact이 **"열어서 바로 이해 가능"**해짐
- SCODA의 핵심 가치("서버 없이 이해 가능")를 UI 계층에서도 실현

---

## 3. Display Intent 스키마 제안

### 3.1 테이블 정의

```sql
CREATE TABLE ui_display_intent (
  id INTEGER PRIMARY KEY,
  entity TEXT NOT NULL,          -- 대상 데이터 개념 (e.g., "genera", "references")
  default_view TEXT NOT NULL,    -- 의미적 뷰 타입 (아래 vocabulary 참조)
  description TEXT,              -- 이 뷰가 왜 적절한지에 대한 설명
  source_query TEXT,             -- ui_queries.name 참조 (optional)
  priority INTEGER DEFAULT 0    -- 여러 intent가 있을 때 순서 (0이 최우선)
);
```

### 3.2 View Type Vocabulary (최소)

| view type | 의미 | 적합한 데이터 |
|-----------|------|-------------|
| `tree` | 계층 구조 탐색 | 분류 체계, 파일 구조, 조직도 |
| `table` | 평면 목록, 검색/정렬/필터 | 종 목록, 참고문헌, 측정값 |
| `detail` | 단일 레코드 상세 보기 | 개별 종 정보, 논문 상세 |
| `map` | 공간 시각화 | 채집 지점, 분포 데이터 |
| `timeline` | 시간축 시각화 | 지질 연대, 출판 연도 |
| `graph` | 관계 네트워크 | 동의어 관계, 인용 네트워크 |

이 vocabulary는 확장 가능하되, 최소 집합은 spec에서 정의해야 한다.

### 3.3 예시 데이터 (Trilobase 기준)

| entity | default_view | description | priority |
|--------|-------------|-------------|----------|
| genera | tree | "Taxonomic hierarchy is the primary organizational structure" | 0 |
| genera | table | "Flat listing for search and filtering" | 1 |
| references | table | "Literature references sorted by year" | 0 |
| synonyms | graph | "Synonym relationships as a network" | 0 |

---

## 4. Semantic UI vs. Literal UI 정리

### 4.1 양자택일이 아니라 계층의 문제

| 접근 | 장점 | 단점 | SCODA 적합성 |
|------|------|------|-------------|
| **Semantic만** (type: tree, table...) | 이식성 높음, 안전, 오래 지속 | 표현력 제한 | 핵심 철학에 가장 부합 |
| **Literal만** (HTML/CSS/JS) | 정확한 시각 제어 | 프레임워크 종속, 보안 위험, 내구성 낮음 | SCODA 철학과 충돌 |
| **계층형** (Intent → Manifest → Assets) | 최소 해석은 항상 가능, 풍부한 표현도 가능 | 설계 복잡도 증가 | 가장 현실적 |

### 4.2 권장 방향

SCODA의 철학("서버 없이 이해 가능해야 한다")을 따르면:
- **Display Intent** (semantic): 필수에 가까움 — 없으면 스키마를 직접 읽어야 함
- **UI Manifest** (declarative): 선택적 — 더 구체적인 뷰 지시가 필요할 때
- **UI Assets** (literal): 선택적, progressive enhancement — 특정 런타임에서만 작동

Literal assets가 없어도 artifact은 완전히 해석 가능해야 한다. Assets는 "더 나은 경험"이지 "필수 경험"이 아니다.

---

## 5. Open Questions에 대한 의견

문서 Section 10의 열린 질문들에 대한 제안:

### "How expressive should the UI manifest be?"
→ Display Intent와 Manifest를 분리하면 해결된다. Intent는 최소 표현(view type + entity + 설명), Manifest는 필요한 만큼 풍부하게(필드 매핑, 정렬 규칙, 필터 조건 등).

### "Should manifests be versioned independently?"
→ **Yes.** 같은 데이터에 대해 UI가 개선될 수 있다. `ui_manifest.version` 필드가 이미 존재하므로, 이를 활용하면 된다. 단, 데이터 자체가 바뀌면 새 SCODA artifact가 되어야 한다는 원칙은 유지.

### "How should multiple manifests coexist?"
→ 두 가지 방안:
1. Display Intent의 `priority` 필드로 기본 뷰 순서를 결정
2. `audience` 필드를 추가하여 대상별 다른 기본 뷰를 제시 (예: researcher, student, general)

### "What level of interactivity is appropriate?"
→ Read-only artifact이므로, 허용 가능한 인터랙션은:
- **Navigation**: 트리 펼치기/접기, 페이지 이동
- **Filtering**: 조건 검색, 정렬
- **Detail view**: 레코드 선택 → 상세 보기
- **View switching**: 같은 데이터에 대해 tree ↔ table 전환

쓰기 작업(편집, 삭제, 추가)은 local overlay의 영역이지 UI manifest의 영역이 아니다.

---

## 6. SCODA Spec과의 관계

현재 `SCODA_v0.1_spec.md`에서 Interface Metadata는 optional이다. Display Intent를 도입하더라도 이 구조를 유지할 수 있다:

| 준수 수준 | Display Intent | UI Manifest | UI Assets |
|-----------|---------------|-------------|-----------|
| **SCODA-Core** | 없어도 됨 | 없어도 됨 | 없어도 됨 |
| **SCODA-Extended** | 강력 권장 (SHOULD) | 선택 (MAY) | 선택 (MAY) |

- Display Intent가 있으면 → artifact이 "열어서 바로 이해 가능"
- 없으면 → 여전히 유효한 SCODA이지만, 스키마를 직접 읽어야 함

기존 spec 구조를 깨지 않으면서 UI 의도를 자연스럽게 통합할 수 있다.

---

## 7. 문서 구조 개선 제안

### 7.1 Section 4 재구성

현재 "Conceptual Model"이 3계층인데, Display Intent를 추가하여 4계층으로 확장:

```
현재:
1. UI Manifest
2. Saved Queries
3. Optional UI Assets

제안:
0. Display Intent (NEW)
1. UI Manifest
2. Saved Queries
3. Optional UI Assets
```

### 7.2 Section 5에 Display Intent 스키마 추가

기존 5.1 (UI Manifest) 앞에 새로운 5.1로 Display Intent 테이블을 추가하고, 기존 항목들을 5.2, 5.3, 5.4로 밀어낸다.

### 7.3 Fallback 동작 명시

Section 6 (Runtime Responsibilities)에 fallback 규칙을 추가:

```
1. UI Assets가 있으면 → Assets로 렌더링
2. UI Assets가 없고 Manifest가 있으면 → Manifest 해석하여 렌더링
3. Manifest도 없고 Display Intent가 있으면 → Intent 기반 기본 뷰 생성
4. Display Intent도 없으면 → 스키마 기반 generic table view
```

이 fallback chain이 명시되면, 각 계층의 역할과 필요성이 자연스럽게 드러난다.

---

## 8. 우선순위

1. **Display Intent 계층 추가** — 핵심 누락 사항, 문서의 가장 큰 개선점
2. **4계층 구조로 Conceptual Model 재구성** — Display Intent 도입에 따른 구조 조정
3. **Fallback 규칙 명시** — 런타임 동작의 예측 가능성 확보
4. **Open Questions 해소 또는 구체화** — 방향이 잡힌 질문들은 답을 반영
5. **View Type Vocabulary 최소 집합 정의** — tree, table, detail, map, timeline, graph
