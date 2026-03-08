# Architecture Document

## 1. 시스템 개요

> 이 시스템이 어떤 구조로 동작하는지 한 문단으로 설명한다.

(작성)

---

## 2. 모듈 구성

> 시스템을 구성하는 모듈과 각 모듈의 역할을 정의한다.

| 모듈명 | 위치 | 역할 |
| --- | --- | --- |
| module_a | `src/module_a.py` | 설명 |
| module_b | `src/module_b.py` | 설명 |

---

## 3. 데이터 흐름

> 입력부터 출력까지 데이터가 어떻게 흐르는지 정의한다.

```
입력
  ↓
module_a  (전처리)
  ↓
module_b  (처리)
  ↓
출력
```

---

## 4. 디렉토리 구조

```
src/
├─ module_a.py
├─ module_b.py
└─ utils.py

tests/
└─ task_tests/
   ├─ test_task1.py
   └─ test_task2.py
```

---

## 5. 의존성

> 외부 라이브러리 및 서비스 의존성을 명시한다.

| 항목 | 버전 | 용도 |
| --- | --- | --- |
| library_a | >= 1.0.0 | 설명 |
| library_b | >= 2.0.0 | 설명 |

---

## 6. 인터페이스 정의

> 모듈 간 주요 인터페이스를 정의한다.

**module_a**
- 입력: `(param: type) -> return_type`
- 역할: 설명

**module_b**
- 입력: `(param: type) -> return_type`
- 역할: 설명
