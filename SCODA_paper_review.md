# SCODA Paper Review

SCODA_paper.md에 대한 평가 및 개선 제안 정리

---

## 1. 강점

### 구조가 명확함
Introduction → Motivation → Objection 반박 → Definition → 기존 연구와의 관계 → Discussion → Conclusion 흐름이 논리적이다.

### 핵심 기여가 분명함
> "a change in governance rather than serialization"

이 문장이 SCODA의 학술적 기여를 정확히 요약한다. 새로운 포맷이 아니라 새로운 거버넌스 모델이라는 점.

### Section 3이 효과적
"Why Not Just Add Metadata?"는 예상되는 반론을 선제적으로 다루는 좋은 전략이다. *expressibility vs enforcement* 구분이 핵심을 잘 짚는다.

### sensu 설명이 개선됨
Section 2.2에서 *sensu*를 "a view over a common factual substrate"로 설명한 것이 비전공자도 이해할 수 있게 해준다.

---

## 2. 개선 필요 사항

### Abstract 길이
현재 약 180단어. 대부분의 저널은 150단어 이하를 선호한다. 마지막 문장(Frictionless, RO-Crate, BagIt 언급)은 본문에서 다루므로 생략 가능.

### References 보완
- RO-Crate, Frictionless Data, BagIt의 URL 또는 DOI 추가
- Boettiger et al. (2015) 논문 제목 확인 필요

---

## 3. 추가 작업 항목

### 3.1 Related Work 보강
현재 Related Work 섹션이 얇다. 다음 내용 추가 필요:

- **FORCE11 Data Citation Principles** - 데이터 인용 표준과 SCODA의 연결
- **FAIR Principles** (Findable, Accessible, Interoperable, Reusable) - SCODA가 FAIR와 어떻게 보완되는지
- **Lakehouse Time Travel** (Delta Lake, Apache Iceberg 등) - 데이터 버전 관리의 기술적 선례

### 3.2 Figure 1 추가
계층 모델을 시각화하는 다이어그램 필요:

```
┌─────────────────────────────────────────────────────┐
│                  Canonical Server                   │
│         (latest view, search, API access)           │
└─────────────────────┬───────────────────────────────┘
                      │ releases
                      ▼
┌─────────────────────────────────────────────────────┐
│                  SCODA Release                      │
│     (versioned, immutable, citable snapshot)        │
└─────────────────────┬───────────────────────────────┘
                      │ extends
                      ▼
┌─────────────────────────────────────────────────────┐
│                  Local Overlay                      │
│   (personal annotations, alternative sensu views)   │
└─────────────────────────────────────────────────────┘
```

양방향 화살표로 서버와 릴리스 간의 관계, 오버레이의 독립성을 표현할 것.

### 3.3 Trilobase Case Section (1페이지)
실증 사례 섹션 추가. 포함할 내용:

- Trilobase 프로젝트 개요 (trilobite genus-level taxonomy)
- SCODA 5개 컴포넌트가 Trilobase에서 어떻게 구현되는지
- 실제 SCODA 릴리스 예시 (버전, 해시, 메타데이터 구조)
- sensu 개념이 실제로 어떻게 표현되는지

### 3.4 Venue 맞춤 톤 조정
타겟 저널/트랙에 따라 톤 조정 필요:

| Venue | 조정 방향 |
|-------|----------|
| **Palaeontologia Electronica (PE)** | 분류학 맥락 강조, sensu 사례 확대 |
| **Methods in Ecology and Evolution (MEE)** | 방법론적 기여 강조, 재현성 문제 부각 |
| **Datasets & Benchmarks 트랙** (NeurIPS/ICML 등) | 기술적 정의 정밀화, 벤치마크 가능성 언급 |

---

## 4. 투고 대상 저널 후보

- **Palaeontologia Electronica (PE)** - 고생물학 오픈 액세스, 분류학 데이터에 적합
- **Methods in Ecology and Evolution (MEE)** - 방법론 논문, 높은 임팩트
- **Journal of Open Research Software** - 오픈 소스/데이터 인프라
- **Data Science Journal** - 데이터 관리 정책
- **PeerJ Computer Science** - 빠른 리뷰, 오픈 액세스
- **Scientific Data** (Nature) - 데이터 기술 논문

---

## 5. 우선순위

1. **Trilobase case section** - 실증 없이는 순수 개념 제안에 그침
2. **Related Work 보강** - 학술적 맥락 확립
3. **Figure 1** - 핵심 아이디어 시각화
4. **Venue 선정 후 톤 조정**
