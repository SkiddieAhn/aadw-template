# 프로젝트 규칙

## 이 프로젝트의 개발 방식
이 프로젝트는 Document-driven AI development workflow 방식으로 진행된다.
사람은 Human Document로 요구사항과 스펙을 정의하고, 각 단계의 검증 게이트를 담당한다.
AI는 각 단계를 실행하고 보고한다.

## 참고 문서 구조
- document/    : 사람이 작성한 Human Document
- docs/        : AI가 생성한 개발 문서 (prd, spec, architecture, roadmap)
- template/ : 각 문서 작성을 위한 빈 양식 모음
- tasks/       : Task 목록과 진행 상태
- .ai-context/implementation-context.md : 현재 프로젝트 상태 요약

## 문서 생성 규칙
docs/ 및 tasks/ 하위 문서를 생성할 때는 반드시 template/ 의 해당 양식을 참고한다.

| 생성할 문서 | 참고 템플릿 |
|---|---|
| docs/prd.md | template/prd.template.md |
| docs/spec.md | template/spec.template.md |
| docs/architecture.md | template/architecture.template.md |
| docs/roadmap.md | template/roadmap.template.md |
| tasks/tasks.md | template/tasks.template.md |
| .ai-context/implementation-context.md | template/implementation-context.template.md |

## Task 완료 규칙
Task 구현이 끝나면 반드시 아래 순서를 따른다.

1. Task 시작 시:
   - tasks.md: TODO → IN PROGRESS
   - implementation-context.md: 현재 상태 반영
2. Task N 테스트 실행
3. Task 1 ~ N-1 테스트 재실행 (이전 기능이 망가지지 않았는지 확인)
4. 전부 통과하면 완료 보고 (문서 업데이트는 하지 않음)
5. 실패하면 원인 파악 및 수정 후 다시 테스트 (3회 반복 실패 시 사람에게 에스컬레이션)
6. 사람이 직접 검증 후 승인하면:
   - tasks.md: IN PROGRESS → DONE + 결과 기록 (생성된 파일, 테스트 결과, 특이사항)
   - implementation-context.md: 현재 상태 반영
7. 사람이 재작업을 요청하면 IN PROGRESS 유지, 수정 후 재시도

**원칙**: 사람의 승인이 완료되기 전까지 Task 상태를 DONE으로 변경하지 않는다.
문서는 항상 사람이 검증한 상태만 반영한다.

## tasks.md 업데이트 형식
Task 상태는 다음 세 가지만 사용한다: 📋 TODO / 🔄 IN PROGRESS / ✅ DONE
- 작업 시작 시: TODO → IN PROGRESS
- 사람 승인 후: IN PROGRESS → DONE (이때만 나머지 항목을 채워 넣음)

```
## Task N: [제목]

**상태**: ✅ DONE
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
Task 시작 시, 사람 승인 후 두 시점 모두 업데이트한다.
아래 항목을 유지하며 현재 상태를 반영한다.

```
## 프로젝트 한 줄 설명
[프로젝트 설명]

## 주요 모듈
- [모듈명]: [역할 설명]

## 주의사항
- [주의사항]

## 현재 상태
- Task 1 ✅ DONE / Task 2 🔄 IN PROGRESS / Task 3 📋 TODO
- 현재 진행 중: Task N (구현 중 / 사람 검증 대기 중)

## 지금까지 결정한 것들
- [결정 사항]: [Task N에서 적용한 내용]
```

## 공개 문서 작성 규칙
PR description, README 등 외부 공개 문서는
사람이 명시적으로 요청한 경우에만 작성한다.
자동으로 생성하지 않는다.