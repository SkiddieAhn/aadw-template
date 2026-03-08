# 프로젝트 규칙

## 이 프로젝트의 개발 방식
이 프로젝트는 Document-driven AI development workflow 방식으로 진행된다.
사람은 Human Document로 요구사항과 스펙을 정의하고, 각 단계의 검증 게이트를 담당한다.
AI는 각 단계를 실행하고 보고한다.

## 참고 문서 구조
- document/ : 사람이 작성한 Human Document
- docs/      : AI가 생성한 개발 문서 (prd, spec, architecture, roadmap)
- tasks/     : Task 목록과 진행 상태
- .ai-context/implementation-context.md : 현재 프로젝트 상태 요약

## Task 완료 규칙
Task 구현이 끝나면 반드시 아래 순서를 따른다.

1. Task N 테스트 실행
2. Task 1 ~ N-1 테스트 재실행 (이전 기능이 망가지지 않았는지 확인)
3. 전부 통과하면:
   - tasks.md에 결과 기록 (상태, 생성된 파일, 테스트 결과, 특이사항)
   - .ai-context/implementation-context.md 현재 상태 업데이트
   - 완료 보고
4. 실패하면:
   - 원인 파악 및 수정 후 다시 테스트
   - 테스트가 통과하기 전까지 문서 업데이트 금지

## 문서 작성 규칙
PR description, README 등 외부 공개 문서는
사람이 명시적으로 요청한 경우에만 작성한다.
자동으로 생성하지 않는다.

## tasks.md 업데이트 형식
Task 완료 시 아래 스키마를 정확히 따라 기록한다.

```
## Task N: [제목]

**상태**: ✅ 완료
**참고 문서**: `docs/spec.md` [절], `docs/architecture.md` [절]
**선행 Task**: [선행 Task 목록 또는 없음]

### 구현 내용
- [구현한 내용]

### 완료 기준
- [x] [기준 1]
- [x] [기준 2]
- [x] tests/task_tests/test_taskN.py 전부 통과

### 생성된 파일
- [파일 경로]

### 테스트 결과
- Task N 테스트: N/N 통과
- 이전 Task 테스트 (Task 1~N-1): 전부 통과

### 특이사항
[특이사항 없으면 "없음"]
```

## implementation-context.md 업데이트 형식
Task 완료 시 아래 항목을 유지하며 현재 상태를 반영한다.

```
## 프로젝트 한 줄 설명
[프로젝트 설명]

## 주요 모듈
- [모듈명]: [역할 설명]

## 주의사항
- [주의사항]

## 현재 상태
- Task 1 ✅ / Task 2 ✅ / ...
- 현재 진행 중: Task N 또는 "전체 완료"

## 지금까지 결정한 것들
- [결정 사항]: [Task N에서 적용한 내용]
```
