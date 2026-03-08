# Tasks

## Task 1: 입력 파일 읽기 구현

**상태**: ✅ 완료
**참고 문서**: `docs/spec.md` 1절, `docs/architecture.md` 1절
**선행 Task**: 없음

### 구현 내용
- YAML 형식의 시나리오 파일을 불러와서 파싱
- 파일 경로 유효성 검사 포함

### 완료 기준
- [x] spec.md에 정의된 입력 형식을 정확히 따름
- [x] tests/task_tests/test_task1.py 전부 통과

### 생성된 파일
- src/scenario_loader/loader.py
- src/scenario_loader/validator.py

### 테스트 결과
- Task 1 테스트: 8/8 통과
- 이전 Task 테스트: 없음 (첫 번째 Task)

### 특이사항
없음

---

## Task 2: 검증 실행 구현

**상태**: ✅ 완료
**참고 문서**: `docs/spec.md` 2절, `docs/architecture.md` 2절
**선행 Task**: Task 1 (입력 파일 읽기)

### 구현 내용
- 규칙에 따라 입력값 검사
- 검증 결과를 구조화된 형태로 반환

### 완료 기준
- [x] spec.md에 정의된 검증 규칙을 전부 구현
- [x] Task 1의 출력을 입력으로 받아 정상 동작
- [x] tests/task_tests/test_task2.py 전부 통과

### 생성된 파일
- src/validation_engine/engine.py
- src/validation_engine/rules.py

### 테스트 결과
- Task 2 테스트: 15/15 통과
- 이전 Task 테스트 (Task 1): 전부 통과

### 특이사항
없음

---

## Task 3: 결과 파일 생성기 구현

**상태**: ✅ 완료
**참고 문서**: `docs/spec.md` 3.2절, `docs/architecture.md` 2절
**선행 Task**: Task 1 (입력 파일 읽기), Task 2 (검증 실행)

### 구현 내용
- 검증 결과를 보기 좋은 형태로 묶어서 파일로 저장
- JSON과 YAML 형식 모두 지원

### 완료 기준
- [x] spec.md에 정의된 출력 형식을 정확히 따름
- [x] Task 2의 결과물을 입력으로 받아 정상 동작
- [x] tests/task_tests/test_task3.py 전부 통과

### 생성된 파일
- src/result_generator/builder.py
- src/result_generator/writer.py

### 테스트 결과
- Task 3 테스트: 12/12 통과
- 이전 Task 테스트 (Task 1~2): 전부 통과

### 특이사항
YAML로 저장할 때 특수문자가 포함된 경우 깨지는 문제가 있어서 별도 처리 추가함

---

## Task 4: 점수 계산 구현

**상태**: 🔲 대기 중
**참고 문서**: `docs/spec.md` 4절, `docs/architecture.md` 3절
**선행 Task**: Task 3 (결과 파일 생성기)

### 구현 내용
- 검증 결과를 기반으로 최종 점수 산출
- 점수 리포트 파일 생성

### 완료 기준
- [ ] spec.md에 정의된 점수 계산 공식을 정확히 따름
- [ ] Task 3의 결과물을 입력으로 받아 정상 동작
- [ ] tests/task_tests/test_task4.py 전부 통과

### 생성된 파일
(완료 후 AI가 채워 넣음)

### 테스트 결과
(완료 후 AI가 채워 넣음)

### 특이사항
(완료 후 AI가 채워 넣음)
