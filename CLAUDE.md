# 프로젝트 규칙

이 프로젝트는 **Document-driven AI development workflow** 방식으로 진행된다.

* 사람: Human Document 작성, 요구사항 정의, 검증 게이트 담당
* AI: 각 단계 실행 및 결과 보고

AI는 항상 **아래 정의된 Task 실행 사이클을 따른다.**
단계를 건너뛰거나 순서를 변경하지 않는다.

---

## 언어 정책

| 대상 | 언어 |
| --- | --- |
| 코드 (변수명, 함수명, 클래스명) | 영어 |
| 코드 주석 | 영어 |
| 커밋 메시지 | 영어 |
| 모든 문서 (.md) | 한국어 |
| 로그 / 에러 메시지 | 영어 |

---

# 개발 환경 규칙

코드를 처음 작성할 때는 사용자가 가상환경을 별도로 지정하지 않는 한,
반드시 `uv venv`를 통해 가상환경을 구축한 후 진행한다.

```
uv venv
source .venv/bin/activate  # Windows: .venv\Scripts\activate
```

---

# 디렉토리 구조

```
project-root/
├── ai/                  — Claude 협업 아티팩트 (문서, 템플릿, 태스크)
│   ├── document/        — 사람이 작성한 Human Document (요구사항 원본)
│   ├── docs/            — AI가 생성한 개발 문서 (prd, spec, architecture, roadmap)
│   ├── examples/        — AADW 워크플로우 참고 예시 프로젝트
│   ├── tasks/           — Task 목록과 진행 상태
│   └── template/        — 각 문서 작성을 위한 빈 양식 모음
├── tests/               — 테스트 코드 및 테스트 보고서
├── src/               — 프로젝트 주요 코드
└── CLAUDE.md
```

협업 문서(`ai/`)와 구현 코드는 분리한다.
AI는 협업 아티팩트를 `ai/` 하위에만 생성하고, 구현 파일은 최상위 경로에 둔다.

---

# 참고 문서 구조

* `ai/document/` : 사람이 작성한 Human Document
* `ai/docs/` : AI가 생성한 개발 문서 (prd, spec, architecture, roadmap)
* `ai/template/` : 각 문서 작성을 위한 빈 양식 모음
* `ai/tasks/` : Task 목록과 진행 상태
* `tests/` : 테스트 코드 및 테스트 보고서
* `src/` : 프로젝트 주요 코드 
* `.ai-context/implementation-context.md` : 현재 프로젝트 상태 요약

---

# 문서 생성 규칙

ai/docs/ 및 ai/tasks/ 하위 문서를 생성할 때는 반드시 `ai/template/` 의 해당 양식을 참고한다.

| 생성할 문서 | 참고 템플릿 |
| --- | --- |
| ai/docs/prd.md | ai/template/prd.template.md |
| ai/docs/spec.md | ai/template/spec.template.md |
| ai/docs/architecture.md | ai/template/architecture.template.md |
| ai/docs/roadmap.md | ai/template/roadmap.template.md |
| ai/tasks/tasks.md | ai/template/tasks.template.md |
| .ai-context/implementation-context.md | ai/template/implementation-context.template.md |

`ai/template/` 폴더가 존재하지 않는 경우, 사람에게 먼저 알린다.
사람의 허가에 따라 ai/docs 내 유사 문서를 참고하여 작성하거나 표준 구조에 맞춰 제작될 수 있다.

---

# 문서 구성

### 1. `prd.md` — 무엇을 만드는가

- 이 라이브러리/모듈이 해결하는 **문제 상황**과 **핵심 기능 목록**을 설명한다.
- 코드를 전혀 모르는 상태에서 읽어도 이해할 수 있도록 비유와 예시를 풍부하게 쓴다.
- 구성:
  - 한 줄 요약
  - 문제 상황 (왜 필요한가)
  - 핵심 기능 요약 표 (기능명 / 설명 / 진입점 / 관련 코드)
  - 기능별 상세 설명 (F1, F2, ...)
  - 어디서 사용되는가
  - 의존하는 외부 서비스

### 2. `architecture.md` — 어떻게 만들어져 있는가

- 코드베이스의 **구조와 흐름**을 top-down으로 설명한다.
- 구성 순서: **진입점 → 디렉토리 구조  → 디자인패턴 → 유즈케이스 → 코드 예시**
  1. **진입점**: 외부에서 어떻게 호출하는가 (HTTP API 엔드포인트, 내장 import 경로 등)
  2. **디렉토리 구조**: 전체 파일 트리 + 한 줄 설명
  3. **핵심 디자인패턴**: 이 코드베이스에서 쓰인 주요 패턴 (추상 설명 → 코드 적용 예)
  4. **유즈케이스별 시퀀스 다이어그램**: 액터 목록 표 + 유즈케이스 목록 표 + Mermaid 다이어그램
  5. **코드 예시**: 핵심 파일별 주요 스니펫 

### 3. `spec.md` — 클래스/인터페이스 상세

- 직접 호출하게 되는 클래스 위주로 **Mermaid 클래스 다이어그램 + 필드/메서드 표 + 계약 표**를 제공한다.
- 구성:
  - 포커스 범위: 디렉토리 구조에서 ★로 어떤 클래스를 다루는지 명시
  - 클래스별 섹션:
    1. 클래스 다이어그램 (Mermaid)
    2. 필드 표 (이름 / 타입 / 설명)
    3. 메서드 표 (시그니처 / 호출 시점 / 설명)
    4. 입출력 표 (파라미터명 / 타입 / 설명)
    5. 계약 표:
       - 입력 제약조건: 유효한 입력의 전제 조건 (예: `doc_index >= 0`, `question` 비어있으면 안 됨)
       - 반환 보장: 성공 시 반환값의 형태와 보장 사항
       - 예외/에러 조건: 어떤 상황에서 예외가 발생하거나 빈 결과를 반환하는가
    6. 사용 예시: 이 클래스를 실제로 어떻게 호출하는가 (내부 구현이 아닌 호출자 관점)

---

# Task 실행 사이클

모든 Task는 다음 lifecycle을 따른다.

```
Task N 할당                                              ← 사람
      ↓
tasks.md: TODO → IN PROGRESS                             ← AI
implementation-context.md: 현재 상태 반영                ← AI
      ↓
Task N 테스트 코드 작성 (Test-first)                      ← AI
      ↓
Task N 구현                                              ← AI
      ↓
Task N 테스트 실행                                        ← AI
      ↓
Task N 테스트 결과 보고서 작성                            ← AI
      ↓
이전 Task 테스트 재실행 (Task 1 ~ N-1)                    ← AI
      ↓
    ┌─ 실패 ──→ 원인 파악 및 수정 → 테스트 재실행          ← AI
    │           (3회 반복 실패 시 사람에게 에스컬레이션)
    │
    └─ 전부 통과
            ↓
         완료 보고 → 사람에게 전달                        ← AI
            ↓
         사람이 직접 실행 및 검증                         ← 사람
            ↓
    ┌─ 재작업 필요 ──→ IN PROGRESS 유지 후 수정           ← 사람
    │
    └─ 승인
            ↓
tasks.md: IN PROGRESS → DONE                              ← AI
implementation-context.md 업데이트                        ← AI
```

---

## 에스컬레이션 조건

아래 상황에서는 즉시 작업을 중단하고 사람에게 보고한다. 임의로 판단하여 진행하지 않는다.

| 조건 | 설명 |
| --- | --- |
| 테스트 3회 연속 실패 | 원인 파악 후 수정했으나 통과하지 못한 경우 |
| Spec 모호 또는 충돌 | spec.md의 규칙이 불명확하거나 서로 상충하는 경우 |
| 설계 결정 필요 | architecture.md에 없는 구조적 판단이 필요한 경우 |
| 외부 의존성 문제 | 라이브러리, API, 환경 등 AI가 해결할 수 없는 외부 요인 |

보고 형식:

```
[에스컬레이션]
조건: [위 조건 중 해당 항목]
상황: [구체적인 상황 설명]
시도한 것: [AI가 시도한 내용]
필요한 것: [사람에게 필요한 판단 또는 정보]
```

---

## Task 의존성 및 재사용 원칙

Task 간 의존성이 있는 경우 다음을 반드시 따른다.

* 이전 Task의 구현 결과를 반드시 재사용한다.
* 동일 기능을 각 Task에서 중복 구현하지 않는다.
* Task는 독립적으로 보이더라도 실제 구현은 누적 구조를 따른다.

테스트 시에도 동일 원칙을 적용한다.

* 이전 Task의 결과를 기반으로 테스트를 수행한다.
* 각 Task를 완전히 독립적으로 테스트하기 위해 기존 구현을 무시하는 행위를 금지한다.

위 원칙을 위반하는 경우, 해당 Task는 올바르게 수행된 것으로 간주하지 않는다.

---

# 테스트 작성 규칙

모든 Task는 **Test-first 방식**으로 진행한다.

순서:

1. 테스트 코드 작성
2. 기능 구현
3. 테스트 실행
4. 테스트 결과 보고서 작성

구현 전에 테스트를 실행하면 **테스트 실패가 정상이다.**

---

# 테스트 코드 규칙

각 Task는 반드시 전용 테스트 파일을 가진다.

예시

```
tests/task_tests/test_task1.py
tests/task_tests/test_task2.py
```

테스트는 다음을 검증해야 한다.

* 정상 입력 처리
* 경계값 처리
* 오류 처리
* 이전 Task와의 호환성

---

# 테스트 픽스처 규칙

각 Task의 테스트는 반드시 `test_save_fixture` 테스트를 포함한다.

* 더미 입력 데이터와 변환/실행 결과를 `tests/fixtures/taskN/` 에 저장한다.
* 저장된 파일은 사람이 직접 열어서 결과를 확인할 수 있어야 한다.
* 텍스트로 읽기 어려운 포맷(parquet 등)이 있을 경우 사람이 읽을 수 있는 포맷(JSON, CSV 등)으로 함께 저장한다.

예시

```
tests/fixtures/task4/
├── dummy.parquet        — 입력 더미 데이터
├── raw.jsonl            — 변환 결과 전체
└── raw_pretty.json      — 상위 N개 pretty-print (사람 확인용)
```

픽스처 디렉토리는 테스트 실행 시마다 덮어쓴다.

픽스처 결과는 반드시 테스트 결과 보고서(`tests/reports/taskN-test-report.md`)에도 포함한다.

* 픽스처 파일 목록과 경로를 명시한다.
* 출력 샘플(첫 번째 항목 등)을 보고서 본문에 직접 인용한다.
* system prompt는 길이(chars)만 기록하고 전문은 생략한다.

---

# 테스트 결과 보고서 규칙

AI는 테스트 실행 후 반드시 **테스트 결과 보고서**를 작성한다.

경로 예시

```
tests/reports/task1-test-report.md
tests/reports/task2-test-report.md
```

보고서에는 다음 내용을 포함한다.

### 테스트 목적

이 테스트가 무엇을 검증하는지 설명

### 테스트 입력

사용된 입력 데이터

### 데이터 출처

다음 중 하나를 명시한다.

* 사용자 제공 데이터
* 더미 데이터 생성

### 테스트 입력 데이터 규칙

테스트 실행 시 사용할 입력 데이터는 다음 우선순위를 따른다.

1️⃣ **사용자가 제공한 실제 데이터가 있는 경우**

* 해당 데이터를 사용하여 테스트를 실행한다.

2️⃣ **사용자 데이터가 없는 경우**

* 테스트 목적에 맞는 **더미 데이터(dummy data)** 를 생성하여 테스트를 실행한다.

더미 데이터는 다음 기준을 따른다.

* 테스트 목적을 충족해야 한다
* 정상 입력 / 경계값 / 오류 케이스를 포함해야 한다
* 실제 사용 상황을 가능한 한 현실적으로 모사한다

---

### 예상 결과

정상 동작 시 기대 결과

### 실제 실행 결과

테스트 실행 결과

### 테스트 결과

* PASS / FAIL
* 통과 개수

### 테스트 실행 명령

```
pytest tests/task_tests/test_taskN.py
```

### pytest 실행 로그

```
============================= test session starts =============================
collected N items

tests/task_tests/test_taskN.py .........

============================== N passed in 0.XXs ==============================
```

### 테스트 코드 위치

```
tests/task_tests/test_taskN.py
```

### 비고

특이사항 또는 테스트 설계 의도

---

**원칙**

테스트 결과 보고서에는 반드시 다음이 포함되어야 한다.

* 테스트 실행 명령
* pytest 실행 로그
* 테스트 입력 데이터
* 데이터 출처

이 정보가 없는 경우 **테스트가 실제 실행된 것으로 간주하지 않는다.**

---

# Task 완료 규칙

모든 Task 완료 판단은 Task 실행 사이클을 기준으로 한다.

---

**원칙**

사람의 승인이 완료되기 전까지
Task 상태를 **DONE으로 변경하지 않는다.**

문서는 항상 **사람이 검증한 상태만 반영한다.**

---

# tasks.md 업데이트 형식

Task 상태는 세 가지만 사용한다.

📋 TODO / 🔄 IN PROGRESS / ✅ DONE

* 작업 시작 시
  TODO → IN PROGRESS

* 사람 승인 후
  IN PROGRESS → DONE

```
## Task N: [제목]

상태: ✅ DONE

참고 문서:
ai/docs/spec.md [절]
ai/docs/architecture.md [절]

선행 Task:
[선행 Task 목록 또는 없음]

### 구현 내용
- [구현 내용]

### 완료 기준
- [x] 기준 1
- [x] 기준 2
- [x] tests/task_tests/test_taskN.py 전부 통과

### 생성된 파일
- [파일 경로]

### 테스트 결과
- Task N 테스트: N/N 통과
- 이전 Task 테스트: 전부 통과

### 특이사항
없음
```

---

# implementation-context.md 업데이트 형식

Task 시작 시 / 사람 승인 후
두 시점 모두 업데이트한다.

```
## 프로젝트 한 줄 설명
[프로젝트 설명]

## 주요 모듈
- [모듈]: 역할 설명

## 주의사항
- [주의사항]

## 현재 상태
Task 1 ✅ DONE
Task 2 🔄 IN PROGRESS
Task 3 📋 TODO

현재 진행 중:
Task N (구현 중 / 사람 검증 대기)

## 지금까지 결정한 것들
- [결정 사항]
```

---

# 공개 문서 작성 규칙

PR description, README 등 **외부 공개 문서**는
사람이 명시적으로 요청한 경우에만 작성한다.

AI가 자동으로 생성하지 않는다.
