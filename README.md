# AI-Assisted Development Workflow (AADW)
Licensed under the **MIT License** - see the [LICENSE](LICENSE) file for details.

> [!IMPORTANT]
> AI 코딩 어시스턴트의 등장으로 개발자의 역할이 전환되고 있다.
> '직접 코드를 타이핑하는 사람'에서 '설계자이자 코드 리뷰어(QA)'로.
> 이 문서는 그 전환을 지원하는 AI 협업 개발 가이드다.

<img width="700" height="350" alt="image" src="https://github.com/user-attachments/assets/d8a97856-2a88-4377-8ee4-e0e1c437cefc"/>  

## 개요

이 문서는 **AI-Assisted Development Workflow(AADW)** 를 정의한다.

AADW는 사용자 정의에 따라 다양한 방식으로 구성될 수 있지만,
이 문서에서는 **Document-driven, Task-based, Human-verify** 방식을 기반으로 한 개발 워크플로우를 설명한다.

1. 사람은 Human Document를 통해 요구사항과 스펙을 정의한다.
2. AI는 이를 기반으로 개발 문서를 생성하고 작업을 Task 단위로 수행한다.
3. 사람은 각 Task 단계에서 검증 게이트(verification gate) 역할을 수행하여 결과를 확인하고 필요에 따라 방향을 조정한다.

이러한 방식을 사용하는 이유는 다음과 같다.

1. 모든 개발 과정을 AI에게 전적으로 맡길 경우, 작업 난이도에 따라 결과의 품질이 크게 달라질 수 있으며 결과를 예측하기 어렵다.
2. Task 단위로 작업을 분리하면 각 단계별 검증이 용이하고, PR을 명확하게 남길 수 있으며, 개발 계획을 체계적으로 세울 수 있다.
3. 현재의 AI는 스스로 생성한 결과를 충분히 신뢰할 수준으로 검증하기에는 아직 한계가 있다.

AI는 사람이 설계한 만큼만 잘 만든다. 프롬프트 한 줄로 좋은 결과를 기대하는 건 AI를 믿는 게 아니라 사람의 설계 부재를 AI에게 떠넘기는 것이다. AADW는 그 반대를 택한다. **사람이 설계와 검증에 집중하고, AI는 그 틀 안에서 실행한다.**

---

## 도구 기반

이 워크플로우는 **Claude Code**를 기반으로 작성되었다.

Claude Code는 터미널에서 실행되는 Anthropic의 AI 코딩 툴로, 프로젝트 루트의 `CLAUDE.md`를 자동으로 읽고 작업 컨텍스트로 활용한다. 이 레포의 구조와 명령어 예시는 모두 Claude Code 사용을 전제로 한다.

> Cursor 등 다른 AI 코딩 툴에서도 동일한 워크플로우를 적용할 수 있다.
> 단, `CLAUDE.md`를 읽는 방식이 툴마다 다를 수 있으므로 해당 툴의 컨텍스트 파일 규칙을 확인한다.

---

## 잘 맞는 사람 / 팀 유형

| 유형 | 특징 |
|---|---|
| 🗂️ 정리형 개발자 | 코드 전에 구조와 요구사항을 정리하는 걸 선호 · PRD / 설계 문서 작성에 거부감이 없음 |
| 👥 작은 제품 팀 (3~10명) | 협업은 필요하지만 대기업 프로세스는 없는 팀 · 공통 문서 기반이 필요한 팀 |
| 🧩 제품 중심 개발자 | 기능 하나보다 전체 제품 구조를 중요하게 보는 사람 · SaaS / 서비스 개발 경험이 많은 팀 |
| 🤖 AI를 개발 보조로 쓰는 팀 | Cursor / Copilot / Claude 같은 AI를 자주 사용 · AI에게 작업을 나눠서 시키는 스타일 |
| 🔁 장기 프로젝트 팀 | 유지보수와 기능 확장이 계속되는 프로젝트 · 온보딩 문서가 필요한 팀 |

---

## 전체 흐름

```
Human Document
      ↓
AI  → PRD / Spec / Architecture
      ↓
Human Verification
      ↓
AI  → Roadmap / Tasks
      ↓
Human Verification
      ↓
AI  → Task-based Implementation
      ↓
Human Verification
```

---

## 핵심 원칙

AADW는 다음 원칙을 따른다.

1. **Document-first** — 모든 개발은 Human Document에서 시작한다.
2. **Spec before code** — 구현 전에 PRD, Spec, Architecture가 먼저 정의된다.
3. **Task-based execution** — 구현은 항상 Task 단위로 진행된다.
4. **Human verification gate** — 각 단계의 결과는 사람이 검증한다.

---

## 역할 분리

| 역할 | 하는 일 |
|---|---|
| 사람 | Human Document 작성 (요구사항 및 스펙 정의), 각 단계 결과물 검증 및 승인 |
| AI | 개발 문서 생성, Roadmap/Tasks 생성, 구현, 테스트, 문서 업데이트, 완료 보고 |

> 사람은 초기 스펙을 결정하고, 각 단계의 검증 게이트를 담당한다.
> AI는 할당받은 Task 하나를 끝내고 보고하는 것까지만 책임진다.
> 다음 Task로 진행할지는 항상 사람이 결정한다.

---

## Human Document

Human Document는 AI가 개발 문서를 생성하는 기반이 되는 사람 중심의 요구사항 문서다.

```
설명 + 계약 + 규칙 + 예시
```

### 최소 포함 요소

Human Document는 아래 항목을 반드시 포함해야 한다.

| 항목 | 설명 |
|---|---|
| 시스템 목적 | 무엇을 만들고 어떤 문제를 해결하는지 |
| 입력 데이터 정의 | 시스템이 받는 입력 데이터 구조 |
| 출력 결과 정의 | 시스템이 생성해야 하는 결과 또는 artifact |
| 규칙 / 검증 기준 | 시스템이 만족해야 하는 규칙 또는 validation 조건 |
| 기본 실행 흐름 | 시스템이 동작하는 기본적인 처리 흐름 |

### 권장 포함 요소

아래 요소를 포함하면 문서 품질과 AI 이해도가 크게 향상된다.

| 항목 | 설명 |
|---|---|
| 개념 정의 | 시스템에서 사용하는 주요 개념 정의 |
| 데이터 예시 | 입력 / 출력 데이터 예시 |
| 제약 조건 | 시스템이 반드시 만족해야 하는 제약 |
| 테스트 데이터 구조 | 테스트 케이스 구성 방식 |
| 성능 기준 | latency, throughput, pass rate 등 |

---

## AI Documents

Human Document를 기반으로 AI가 생성하는 개발 문서들이다.

| 문서 | 설명 |
|---|---|
| PRD | 무엇을 왜 만드는지 설명하는 기획 문서 (Product Requirements Document) |
| Spec | 입출력 형식, 동작 규칙 등 시스템이 지켜야 할 계약을 정의한 문서 |
| Architecture | 시스템을 어떤 모듈로 나누고 어떻게 연결할지 설계한 문서 |
| Roadmap | 개발을 어떤 순서로 진행할지 단계를 정의한 문서 |
| Tasks | AI가 실제로 구현할 작업 목록. 하나씩 순서대로 진행된다 |

<details>
      
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

### 2. `architecture.md` — 어떻게 만드는가

- 코드베이스의 **구조와 흐름**을 top-down으로 설명한다.
- 구성 순서: **진입점 → 디렉토리 구조  → 디자인패턴 → 유즈케이스 → 코드 예시**
  1. **진입점**: 외부에서 어떻게 호출하는가 (HTTP API 엔드포인트, 내장 import 경로 등)
  2. **디렉토리 구조**: 전체 파일 트리 + 한 줄 설명
  3. **핵심 디자인패턴**: 이 코드베이스에서 쓰인 주요 패턴 (추상 설명 → 코드 적용 예)
  4. **유즈케이스별 시퀀스 다이어그램**: 액터 목록 표 + 유즈케이스 목록 표 + Mermaid 다이어그램
  5. **코드 예시**: 핵심 파일별 주요 스니펫 (파일명:라인번호 명시)

### 3. `spec.md` — 클래스/인터페이스 상세

- 직접 호출하게 되는 클래스 위주로 **Mermaid 클래스 다이어그램 + 필드/메서드 표 + 입출력 표**를 제공한다.
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
       
</details>

---

## Task 실행 사이클

Task 하나를 할당받으면 AI는 아래 순서를 자동으로 수행한다.
테스트가 통과될 때까지의 구현 루프는 AI가 자율적으로 진행하며, 완료 후 사람이 검증 게이트를 담당한다.

```
Task N 할당                                              ← 사람
      ↓
tasks.md: TODO → IN PROGRESS                             ← AI
implementation-context.md: 현재 상태 반영                ← AI
      ↓
Task N 테스트 코드 작성                                    ← AI
      ↓
Task N 구현                                               ← AI
      ↓
Task N 테스트 실행                                         ← AI
      ↓
이전 Task들 (Task 1 ~ N-1) 테스트 재실행                   ← AI
      ↓
    ┌─ 실패 ──→ 원인 파악 및 수정 ──→ 다시 테스트            ← AI
    │           (3회 반복 실패 시 사람에게 에스컬레이션)
    │
    └─ 전부 통과
            ↓
         완료 보고 → 사람에게 전달                          ← AI
            ↓
         사람이 직접 실행 및 검증                           ← 사람
            ↓
    ┌─ 재작업 필요 ──→ IN PROGRESS 유지, 수정 후 재시도       ← 사람
    │
    └─ 승인
            ↓
         tasks.md: IN PROGRESS → DONE + 결과 기록          ← AI
         implementation-context.md: 현재 상태 반영          ← AI
            ↓
         다음 Task 할당 여부 결정                           ← 사람
```

**원칙**: 사람의 승인이 완료되기 전까지 Task 상태를 DONE으로 변경하지 않는다.
문서는 항상 **사람이 검증한 상태**만 반영한다.

---

## 수정 철학 — Human Document만 고치면 나머지는 AI가 정렬한다

> [!NOTE]
> **전체를 직접 수정하지 않는다.**
> Human Document 하나만 고치면 AI가 diff를 보고 영향 범위를 스스로 판단한다.
> 사람은 `ai/document/`만 수정하고, 나머지 문서 갱신과 Task 재정렬은 AI가 자동으로 수행한다.

- **Top-down change propagation**
```
Human Document 수정                       ← 사람
      ↓
기존 AI 문서와 변경점 비교                 ← AI
      ↓
PRD / Spec / Architecture 갱신            ← AI
      ↓
Human Verification                        ← 사람
      ↓
Roadmap / Tasks 갱신                      ← AI
      ↓
Human Verification                        ← 사람
      ↓
영향받는 Task만 재수행                     ← AI
      ↓
Human Verification                        ← 사람
```

<details>
<summary><b>갱신 명령어 보기</b></summary>
<br>

### Step 1 — 변경점 비교 및 문서 갱신 요청

```
ai/document/ 안의 원본 문서가 수정됐어.
기존 ai/docs/prd.md, ai/docs/spec.md, ai/docs/architecture.md와 비교해서
변경된 부분을 파악하고 각 문서를 갱신해줘.
```

> ✅ 갱신된 PRD / Spec / Architecture를 직접 읽고 내용이 맞는지 확인한다.
> 문제가 없으면 Step 2를 요청한다.

---

### Step 2 — Roadmap / Tasks 갱신 요청

```
변경된 ai/docs/를 기반으로
ai/docs/roadmap.md와 ai/tasks/tasks.md에서 영향받는 부분을 갱신해줘.
새로 추가된 Task와 수정이 필요한 Task를 명확히 구분해줘.
```

> ✅ 갱신된 Roadmap과 Task 목록을 확인한다.
> 영향받는 Task 범위가 예상과 다르면 수정을 요청한다.
> 문제가 없으면 Step 3을 요청한다.

---

### Step 3 — 영향받는 Task 재수행 요청

```
ai/tasks/tasks.md에서 변경의 영향을 받은 Task 목록을 확인하고
해당 Task들만 순서대로 재수행해줘.
```

> ✅ 각 Task 완료 보고를 받을 때마다 직접 실행하여 결과를 검증한다.

</details>

---

# 🚀 시작하는 방법 (처음 하는 사람을 위한 가이드)

> 이 워크플로우는 Human Document를 잘 쓸수록, 검증 게이트를 꼼꼼히 통과시킬수록 결과가 좋아진다.
> AI를 믿기 전에 자신의 설계를 먼저 믿어야 한다.

## Step 1. 이 레포 clone하기

```bash
git clone https://github.com/SkiddieAhn/aadw-template.git my-project
cd my-project
```

clone하면 아래 구조가 만들어진다.

```
my-project/
├─ ai/                           ← Claude 협업 아티팩트 (협업 문서와 구현 코드를 분리)
│  ├─ document/                  ← 내가 작성한 Human Document를 여기에 넣는다
│  │
│  ├─ docs/                      ← AI가 Human Document를 읽고 생성하는 개발 문서들
│  │  ├─ prd.md                  ← 무엇을 왜 만드는지 설명 (기획 문서)
│  │  ├─ spec.md                 ← 입출력 형식, 동작 규칙 정의 (계약 문서)
│  │  ├─ architecture.md         ← 모듈 구조, 데이터 흐름 설계 (설계 문서)
│  │  └─ roadmap.md              ← 개발 단계와 순서 정의 (계획 문서)
│  │
│  ├─ template/                  ← 각 문서 작성을 위한 빈 양식 모음
│  │  ├─ prd.template.md
│  │  ├─ spec.template.md
│  │  ├─ architecture.template.md
│  │  ├─ roadmap.template.md
│  │  ├─ tasks.template.md
│  │  └─ implementation-context.template.md
│  │
│  ├─ tasks/
│  │  └─ tasks.md                ← AI가 구현할 작업 목록 + 진행 상태 + 완료 결과
│  │                                (프로젝트가 커지면 roadmap 단계별로 파일을 나눌 수 있다)
│  │
│  └─ examples/                  ← AADW 워크플로우 참고 예시 프로젝트
│
├─ src/                          ← AI가 실제로 작성하는 구현 코드
│
├─ tests/
│  └─ task_tests/                ← Task별 테스트 코드 (AI가 Task마다 자동 생성)
│
├─ artifacts/
│  └─ outputs/                   ← 프로그램 실행 결과 파일 저장 (git ignore 권장)
│
├─ scripts/
│  └─ run_main.py                ← 프로그램 실행 진입점 (사람이 직접 실행하여 결과를 검증할 때도 사용)
│
├─ .ai-context/
│  └─ implementation-context.md  ← 현재 프로젝트 상태 요약 (세션 초기화 시 AI에게 제공)
│
├─ CLAUDE.md                     ← AI가 이 프로젝트에서 항상 따라야 할 규칙 파일
└─ README.md                     ← 지금 읽고 있는 이 파일 (AADW 워크플로우 가이드)
```

이 `README.md`는 AADW 워크플로우 가이드로, 프로젝트 공개용 README가 아니다.
작업을 시작하기 전에 `ai/`로 옮겨 루트를 비워둔다.

```bash
mv README.md ai/
```

이후 루트의 `README.md`는 프로젝트 전용으로 사용한다.

---

## Step 2. Human Document 작성 후 넣기

만들고 싶은 것을 설명한 문서를 작성해서 `ai/document/` 폴더에 넣는다.

```
ai/document/
└─ source-document.pdf   (또는 .md, .txt 등 아무 형식이나 가능)
```

위의 **Human Document 최소 포함 요소**를 기준으로 작성한다.
완벽하지 않아도 괜찮다. 잘 정리된 문서일수록 AI가 더 정확하게 개발 문서를 만들어준다.

---

## Step 3. CLAUDE.md 작성하기

`CLAUDE.md`는 AI가 이 프로젝트에서 항상 따라야 할 규칙을 적어두는 파일이다.
**Claude Code는 작업을 시작할 때 이 파일을 자동으로 읽는다.**

이 레포의 [`CLAUDE.md`](https://github.com/SkiddieAhn/aadw-template/blob/main/CLAUDE.md) 템플릿을 복사해서 프로젝트에 맞게 수정한다.

---

## Step 4. AI에게 작업 요청하기

<details>
<summary><b>4-1. 개발 문서 생성 요청</b></summary>
<br>

```
아래 정보를 기반으로 개발 문서를 생성해줘.

---

## 🔽 사용자 입력 정보 (필요한 내용만 채워서 사용)

### Human Document 위치

* ai/document/ 경로:
* 파일 목록 (선택):

### 데이터 경로 (선택)

* 입력 데이터:
* 출력 데이터:

### 참고해야 할 추가 문서 (선택)

* 예: 외부 스펙, API 문서, 기획 노트 등

### 제약사항 / 추가 요구사항 (선택)

* 예: 반드시 CLI 형태로 구현
* 예: 특정 라이브러리 사용 금지

---

## 🔽 작업 지시

ai/document/ 디렉토리에 있는 Human Document를 모두 읽고 내용을 정확히 이해해줘.

그 다음 아래 규칙을 반드시 지켜서 문서를 생성해줘.

---

### 1. 생성 대상

* ai/docs/prd.md
* ai/docs/spec.md
* ai/docs/architecture.md

---

### 2. 작성 규칙

* 반드시 ai/template/ 디렉토리의 각 템플릿을 기반으로 작성한다
* 문서 구조를 임의로 변경하지 않는다
* Human Document에 없는 내용은 추측해서 작성하지 않는다
* 추가 입력 정보가 있다면 반드시 반영한다

---

### 3. 작성 순서

* PRD → Spec → Architecture 순서로 작성한다
* 각 문서는 이전 문서를 기반으로 일관되게 작성한다

---

### 4. 정확성 규칙

* 입력 데이터, 출력 결과, 규칙은 Human Document와 반드시 일치해야 한다
* 모호한 부분은 임의로 결정하지 말고 TODO 또는 질문 형태로 표시한다

---

### 5. 결과 출력 방식

* 각 문서를 분리해서 출력한다
* 어떤 내용이 어떤 입력 정보(document/ 또는 추가 입력)에서 나온 것인지 추적 가능하게 작성한다

---

### 6. 금지 사항

* 존재하지 않는 파일이나 경로를 가정하지 않는다
* Human Document에 없는 기능을 임의로 추가하지 않는다
* 더미 데이터를 기반으로 spec을 정의하지 않는다

---

## 🔽 완료 후 추가 출력

* 누락된 정보
* 모호한 요구사항
* 추가로 필요한 데이터 또는 문서

```

AI가 문서를 만들면 읽어보고 확인한다.
잘못된 부분이 있으면 피드백을 주고 수정을 요청한다.
문제가 없으면 다음 단계로 넘어간다.

</details>

<details>
<summary><b>4-2. Roadmap + Tasks 생성 요청</b></summary>
<br>

```
ai/docs/prd.md, ai/docs/spec.md, ai/docs/architecture.md를 읽고
ai/docs/roadmap.md와 ai/tasks/tasks.md를 생성해줘.
```

AI가 만든 Roadmap과 Task 목록을 확인한다.
Task의 순서나 내용이 맞지 않으면 수정을 요청한다.
문제가 없으면 다음 단계로 넘어간다.

</details>

<details>
<summary><b>4-3. Task 구현 요청</b></summary>
<br>

```
ai/docs/spec.md와 ai/docs/architecture.md를 참고해서
ai/tasks/tasks.md의 Task 1을 구현해줘.
```

AI는 테스트 작성 → 구현 → 테스트 실행 → 완료 보고를 자동으로 수행한다.
완료 보고를 받으면 아래 4-4를 통해 직접 검증한다.

</details>

<details>
<summary><b>4-4. 사람이 직접 검증하는 방법</b></summary>
<br>

AI가 완료 보고를 해도 사람이 직접 실행해서 결과를 눈으로 확인하는 것을 권장한다.

**테스트 직접 실행**

특정 Task의 테스트만 확인하고 싶을 때:
```bash
python -m pytest tests/task_tests/test_task1.py -v
```

전체 테스트를 한 번에 확인하고 싶을 때:
```bash
python -m pytest tests/task_tests/ -v
```

**프로그램 직접 실행**

실제로 프로그램을 돌려서 결과 파일을 확인하고 싶을 때:
```bash
python scripts/run_main.py
```

실행 결과는 `artifacts/outputs/`에 저장된다.

**검증 결과에 따른 대응**

| 결과 | 대응 |
|---|---|
| 결과가 예상과 같음 | 다음 Task 할당 |
| 결과가 예상과 다름 | AI에게 구체적인 피드백과 함께 재작업 요청 |
| Task 자체를 수정해야 함 | tasks.md의 해당 Task 내용을 수정 후 재요청 |

재작업 요청 예시:
```
Task 1 결과를 직접 실행해봤는데 [문제 내용]이 발생했어.
[원하는 동작]으로 수정해줘.
```

</details>

<details>
<summary><b>4-5. 세션이 끊겼을 때</b></summary>
<br>

Claude Code를 껐다 켜거나 대화가 초기화된 경우 아래 명령어로 다시 시작한다.

```
.ai-context/implementation-context.md를 읽고 현재 상태를 파악해줘.
그 다음 ai/tasks/tasks.md에서 아직 완료되지 않은 Task를 확인해줘.
```

</details>

<details>
<summary><b>4-6. PR description / README 작성 요청</b></summary>
<br>

PR description이나 README 같은 외부 공개 문서는 **사람이 명시적으로 요청할 때만** 작성한다.
AI가 자동으로 생성하지 않는다.

요청 시점에 따라 참고 문서가 달라진다.

| 시점 | 참고 문서 | 결과물 |
|---|---|---|
| Task 완료 후 | `prd.md` + `tasks.md` (해당 Task) | PR description |
| Roadmap 단계 완료 후 | `prd.md` + `roadmap.md` + `tasks.md` | Roadmap 단계 PR / 릴리즈 노트 |
| 전체 완료 후 | `prd.md` + `roadmap.md` + `tasks.md` (전체) | 프로젝트 README |

PR description 요청:
```
ai/docs/prd.md, ai/docs/roadmap.md, ai/tasks/tasks.md를 참고해서
Task 3 완료에 대한 PR description을 작성해줘.
```

프로젝트 README 요청:
```
ai/docs/prd.md, ai/docs/roadmap.md, ai/tasks/tasks.md를 참고해서
프로젝트 README.md를 작성해줘.
```

</details>

---

## 문서 역할 정리

| 문서 | 역할 | 용도 | 주로 읽는 대상 |
|---|---|---|---|
| `ai/docs/prd.md` | 왜 만드는지, 무엇을 만드는지 설명 | 📋 문서 제작용 | 사람 |
| `ai/docs/roadmap.md` | 개발 단계와 순서 | 📋 문서 제작용 | 사람 |
| `ai/docs/spec.md` | 입출력 형식, 동작 규칙 정의 | 🔧 실제 개발용 | AI + 사람 |
| `ai/docs/architecture.md` | 모듈 구조와 데이터 흐름 설명 | 🔧 실제 개발용 | AI + 사람 |
| `ai/tasks/tasks.md` | Task 목록 + 진행 상태 + 완료 결과 | 🔧 실제 개발용 | AI + 사람 |
| `.ai-context/implementation-context.md` | 현재 프로젝트 상태 요약 (세션 초기화 시 사용) | 🔧 실제 개발용 | AI |
| `CLAUDE.md` | AI가 항상 따라야 할 프로젝트 규칙 | 🔧 실제 개발용 | AI |
| `ai/template/` | 각 문서 작성을 위한 빈 양식 모음 | 📋 문서 제작용 | 사람 |

---

<details>
<summary><b>tasks/ 디렉토리 구조</b></summary>
<br>

지금은 `ai/tasks/tasks.md` 하나만 쓰는 것으로 충분하다.
프로젝트가 커져서 Task가 많아지면 **roadmap 단계별로 파일을 나눌 수 있다.**

**roadmap 단계란?**
프로젝트에서 의미 있는 완료 지점을 말한다.
Task가 벽돌 하나라면, roadmap 단계는 방 하나다.

```
roadmap 1단계: 로그인 / 회원가입 완성
      ├─ Task 1: 회원가입 폼 구현
      ├─ Task 2: 로그인 API 연동
      └─ Task 3: 세션 관리 구현

roadmap 2단계: 상품 목록 / 상세 페이지 완성
      ├─ Task 4: 상품 목록 API 연동
      └─ Task 5: 상세 페이지 구현
```

Task가 많아지면 roadmap 단계 이름을 그대로 파일명으로 써서 나눌 수 있다.

```
ai/tasks/
├─ tasks.md                  ← 전체 Task 목록 (초기에는 이것만으로 충분)
├─ phase-1-auth.md           ← roadmap 1단계 세부 Tasks
├─ phase-2-product.md        ← roadmap 2단계 세부 Tasks
└─ backlog.md                ← 나중에 할 것들
```

처음에는 `ai/tasks/tasks.md` 하나로 시작하고, 필요할 때 나누면 된다.

</details>

<details>
<summary><b>tasks.md 작성 형식</b></summary>
<br>

Task를 정의할 때와 완료했을 때 모두 이 파일에 기록한다.
AI는 Task가 끝나면 자동으로 항목들을 채워 넣는다.

→ [tasks.md 예시](https://github.com/SkiddieAhn/aadw-template/blob/main/examples/todo-cli/tasks/tasks.md)

</details>

<details>
<summary><b>.ai-context/implementation-context.md 형식</b></summary>
<br>

AI는 대화가 끊기면 이전 내용을 기억하지 못한다.
이 파일은 세션이 초기화됐을 때 AI에게 건네주는 현황 요약 문서다.
Task가 시작되거나 완료될 때마다 AI가 자동으로 업데이트한다.

→ [implementation-context.md 예시](https://github.com/SkiddieAhn/aadw-template/blob/main/examples/todo-cli/.ai-context/implementation-context.md)

</details>

---

## 문서 정보

| 항목 | 내용 |
|---|---|
| 버전 | v1.3.0 |
| 작성자 | Sunghyun Ahn (SkiddieAhn) |
| 이메일 | skd@yonsei.ac.kr |
