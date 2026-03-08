# CLAUDE.md

이 파일은 AI가 이 프로젝트에서 항상 따라야 할 규칙을 정의한다.
Claude Code, Cursor 같은 AI 코딩 툴은 작업 시작 시 이 파일을 자동으로 읽는다.

---

## 프로젝트 개요

- **프로젝트명**: (작성)
- **목적**: (한 줄 요약)
- **문서 생성 시 참고**: `docs_template/` (각 문서의 형식 및 작성 양식)
- **구현 시 참고**: `docs/spec.md`, `docs/architecture.md`, `docs/roadmap.md`, `tasks/tasks.md`

---

## 워크플로우 규칙

1. 작업 시작 전 반드시 `docs/spec.md`와 `docs/architecture.md`를 읽는다.
2. 구현은 항상 `tasks/tasks.md`의 Task 순서를 따른다.
3. 한 번에 하나의 Task만 진행한다. 다음 Task는 사람의 승인 후 시작한다.
4. Task 완료 전까지 `tasks/tasks.md`와 `implementation-context.md`를 업데이트하지 않는다.

---

## Task 실행 순서

Task를 할당받으면 아래 순서를 자동으로 수행한다.

1. 테스트 코드 작성 (`tests/task_tests/test_taskN.py`)
2. 구현
3. 테스트 실행 (Task N)
4. 이전 Task 테스트 재실행 (Task 1 ~ N-1)
5. 전부 통과 시 → `tasks/tasks.md` 업데이트
6. `implementation-context.md` 업데이트
7. 완료 보고

테스트 실패 시 원인을 파악하고 수정한다. 3회 이상 같은 오류가 반복되면 사람에게 에스컬레이션한다.

---

## 코딩 규칙

- 언어: (Python / TypeScript 등 작성)
- 포맷터: (black / prettier 등 작성)
- 테스트 프레임워크: (pytest / jest 등 작성)
- 모든 함수에 docstring을 작성한다.
- 파일 상단에 해당 파일의 역할을 한 줄 주석으로 명시한다.

---

## 하지 말아야 할 것

- spec.md에 정의되지 않은 입출력 형식을 임의로 변경하지 않는다.
- 사람의 승인 없이 architecture.md의 모듈 구조를 변경하지 않는다.
- PR description, README는 사람이 명시적으로 요청할 때만 작성한다.