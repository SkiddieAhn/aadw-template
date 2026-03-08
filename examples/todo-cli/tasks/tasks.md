# Tasks — Todo CLI

## 진행 상태 범례

| 상태 | 의미 |
| --- | --- |
| ⬜ TODO | 아직 시작하지 않음 |
| 🟡 IN PROGRESS | 현재 진행 중 |
| ✅ DONE | 완료됨 |

---

## Phase 1: 핵심 기능 구현

### Task 1: Todo 항목 추가 기능

- **상태**: ✅ DONE
- **목표**: 사용자가 텍스트를 입력하면 Todo 항목이 생성되어 저장된다
- **구현 범위**:
  - [x] `Todo` 데이터 클래스 정의 (id, title, done, created_at)
  - [x] `add_todo(title: str)` 함수 구현
  - [x] JSON 파일 기반 저장 로직 구현
- **테스트 파일**: `tests/task_tests/test_task1.py`
- **완료 조건**: `add_todo("밥 먹기")` 호출 시 JSON 파일에 항목이 저장됨
- **완료 결과**: `src/storage.py`, `src/models.py` 생성. 전체 테스트 3/3 통과.

---

### Task 2: Todo 목록 조회 기능

- **상태**: ✅ DONE
- **목표**: 저장된 Todo 목록을 전체 또는 미완료 항목만 필터링하여 조회한다
- **구현 범위**:
  - [x] `get_todos(filter: str = "all")` 함수 구현 (`all` / `active` / `done`)
  - [x] 빈 목록일 때 빈 리스트 반환
- **테스트 파일**: `tests/task_tests/test_task2.py`
- **완료 조건**: filter 파라미터에 따라 올바른 항목만 반환됨
- **완료 결과**: `src/todo.py`에 조회 로직 추가. 전체 테스트 6/6 통과.

---

### Task 3: Todo 완료 처리 기능

- **상태**: ✅ DONE
- **목표**: 특정 id의 Todo 항목을 완료 상태로 변경한다
- **구현 범위**:
  - [x] `complete_todo(todo_id: int)` 함수 구현
  - [x] 존재하지 않는 id 입력 시 예외 처리
- **테스트 파일**: `tests/task_tests/test_task3.py`
- **완료 조건**: `complete_todo(1)` 호출 시 해당 항목의 `done`이 `True`로 변경됨
- **완료 결과**: `src/todo.py`에 완료 처리 로직 추가. 전체 테스트 9/9 통과.

---

## Phase 2: CLI 인터페이스 구현

### Task 4: CLI 명령어 연결

- **상태**: ⬜ TODO
- **목표**: `add`, `list`, `done` 명령어를 CLI에서 실행할 수 있다
- **구현 범위**:
  - [ ] `argparse` 기반 CLI 진입점 구현
  - [ ] `add <title>`, `list [--filter]`, `done <id>` 명령어 구현
- **테스트 파일**: `tests/task_tests/test_task4.py`
- **완료 조건**: `python scripts/run_main.py add "운동하기"` 실행 시 항목이 추가됨
- **완료 결과**: (Task 완료 후 AI가 채워 넣음)
