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

# 참고 문서 구조

* `document/` : 사람이 작성한 Human Document
* `docs/` : AI가 생성한 개발 문서 (prd, spec, architecture, roadmap)
* `template/` : 각 문서 작성을 위한 빈 양식 모음
* `tasks/` : Task 목록과 진행 상태
* `tests/` : 테스트 코드 및 테스트 보고서
* `.ai-context/implementation-context.md` : 현재 프로젝트 상태 요약

---

# 문서 생성 규칙

docs/ 및 tasks/ 하위 문서를 생성할 때는 반드시 `template/` 의 해당 양식을 참고한다.

| 생성할 문서 | 참고 템플릿 |
| --- | --- |
| docs/prd.md | template/prd.template.md |
| docs/spec.md | template/spec.template.md |
| docs/architecture.md | template/architecture.template.md |
| docs/roadmap.md | template/roadmap.template.md |
| tasks/tasks.md | template/tasks.template.md |
| .ai-context/implementation-context.md | template/implementation-context.template.md |

`template/` 폴더가 존재하지 않는 경우, 템플릿 없이 문서를 생성하되 사람에게 해당 사실을 먼저 알린다.

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

Task 구현이 끝나면 반드시 아래 순서를 따른다.

### 1️⃣ Task 시작 시

* `tasks.md`: TODO → IN PROGRESS
* `implementation-context.md`: 현재 상태 반영

### 2️⃣ Task N 테스트 실행

### 3️⃣ 이전 Task 테스트 재실행

(Task 1 ~ N-1)

### 4️⃣ 전부 통과 시 완료 보고

(문서 업데이트는 하지 않음)

### 5️⃣ 실패 시

* 원인 분석
* 수정 후 재테스트
* 3회 실패 시 사람에게 에스컬레이션

### 6️⃣ 사람이 검증 후 승인하면

* `tasks.md`: IN PROGRESS → DONE
* 결과 기록
* `implementation-context.md` 업데이트

### 7️⃣ 사람이 재작업 요청 시

* IN PROGRESS 유지
* 수정 후 재시도

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
docs/spec.md [절]
docs/architecture.md [절]

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
