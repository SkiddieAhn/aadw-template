# Tasks

<!-- 상태값은 아래 세 가지만 사용한다 -->
<!-- 📋 TODO: 아직 시작 전 -->
<!-- 🔄 IN PROGRESS: AI가 작업 중 또는 사람 검증 대기 중 -->
<!-- ✅ DONE: 사람이 승인 완료 -->

## Task 1: 할 일 추가 기능 구현

**상태**: ✅ DONE
**참고 문서**: `docs/spec.md` 1절, `docs/architecture.md` 1절
**선행 Task**: 없음

### 구현 내용
- CLI 명령어로 할 일 항목 추가
- 추가된 항목을 JSON 파일에 저장

### 완료 기준
- [x] `todo add "할 일 내용"` 명령어 동작
- [x] 항목이 `data/todos.json`에 저장됨
- [x] tests/task_tests/test_task1.py 전부 통과

### 생성된 파일
- src/commands/add.py
- src/storage/json_store.py
- data/todos.json

### 테스트 결과
- Task 1 테스트: 6/6 통과
- 이전 Task 테스트: 없음 (첫 번째 Task)

### 특이사항
없음

---

## Task 2: 할 일 목록 조회 기능 구현

**상태**: ✅ DONE
**참고 문서**: `docs/spec.md` 2절, `docs/architecture.md` 1절
**선행 Task**: Task 1 (할 일 추가)

### 구현 내용
- 저장된 할 일 목록을 CLI에 출력
- 완료 여부에 따라 상태 표시

### 완료 기준
- [x] `todo list` 명령어 동작
- [x] 완료/미완료 항목 구분 출력
- [x] tests/task_tests/test_task2.py 전부 통과

### 생성된 파일
- src/commands/list.py

### 테스트 결과
- Task 2 테스트: 8/8 통과
- 이전 Task 테스트 (Task 1): 전부 통과

### 특이사항
없음

---

## Task 3: 할 일 완료 처리 기능 구현

**상태**: 🔄 IN PROGRESS
**참고 문서**: `docs/spec.md` 3절, `docs/architecture.md` 2절
**선행 Task**: Task 1 (할 일 추가), Task 2 (목록 조회)

### 구현 내용
- ID를 지정해서 할 일 항목을 완료 처리
- 완료 시각 기록

### 완료 기준
- [ ] `todo done <id>` 명령어 동작
- [ ] 완료 시각이 항목에 기록됨
- [ ] tests/task_tests/test_task3.py 전부 통과

### 생성된 파일
(사람 승인 후 AI가 채워 넣음)

### 테스트 결과
(사람 승인 후 AI가 채워 넣음)

### 특이사항
(사람 승인 후 AI가 채워 넣음)

---

## Task 4: 할 일 삭제 기능 구현

**상태**: 📋 TODO
**참고 문서**: `docs/spec.md` 4절, `docs/architecture.md` 2절
**선행 Task**: Task 3 (완료 처리)

### 구현 내용
- ID를 지정해서 할 일 항목 삭제

### 완료 기준
- [ ] `todo delete <id>` 명령어 동작
- [ ] 삭제 후 목록에서 제거됨
- [ ] tests/task_tests/test_task4.py 전부 통과

### 생성된 파일
(사람 승인 후 AI가 채워 넣음)

### 테스트 결과
(사람 승인 후 AI가 채워 넣음)

### 특이사항
(사람 승인 후 AI가 채워 넣음)