# Implementation Context — Todo CLI

> 이 파일은 세션이 초기화됐을 때 AI에게 건네주는 현황 요약 문서다.
> Task가 완료될 때마다 AI가 자동으로 업데이트한다.

---

## 프로젝트 개요

- **프로젝트명**: Todo CLI
- **목적**: 터미널에서 동작하는 간단한 할일 관리 도구
- **기술 스택**: Python 3.11, argparse, JSON 파일 저장

---

## 현재 상태

- **마지막 완료 Task**: Task 3 — Todo 완료 처리 기능
- **다음 Task**: Task 4 — CLI 명령어 연결
- **전체 진행률**: 3 / 4 완료

---

## 완료된 Task 요약

| Task | 이름 | 주요 구현 내용 |
| --- | --- | --- |
| Task 1 | Todo 항목 추가 기능 | `src/models.py` (Todo 데이터 클래스), `src/storage.py` (JSON 저장 로직) |
| Task 2 | Todo 목록 조회 기능 | `src/todo.py`에 `get_todos(filter)` 구현, all/active/done 필터 지원 |
| Task 3 | Todo 완료 처리 기능 | `src/todo.py`에 `complete_todo(id)` 구현, 존재하지 않는 id 예외 처리 |

---

## 현재 디렉토리 구조 (src/)

```
src/
├─ models.py      ← Todo 데이터 클래스 (id, title, done, created_at)
├─ storage.py     ← JSON 파일 읽기/쓰기
└─ todo.py        ← add_todo / get_todos / complete_todo 핵심 로직
```

---

## 알려진 이슈 / 특이사항

- `artifacts/outputs/todos.json`이 없으면 자동 생성되도록 구현됨
- Task 4에서 argparse 연결 시 `scripts/run_main.py`를 진입점으로 사용할 것

---

## 세션 복구 시 AI에게 전달할 명령

```
.ai-context/implementation-context.md를 읽고 현재 상태를 파악해줘.
그 다음 tasks/tasks.md에서 아직 완료되지 않은 Task를 확인해줘.
```
