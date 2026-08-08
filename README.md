<<<<<<< HEAD
# noCode
noCode codyssey

## 프로젝트 
=======
# 노코드 자동화 — 도구 비교 구현 및 자유 주제 파이프라인

## 프로젝트 개요

동일한 워크플로우를 Make와 Zapier 두 도구로 구현해 비교 분석하고(프로젝트 1), 별도의 반복 업무를 자동화하는 파이프라인을 설계·구현한(프로젝트 2) 프로젝트입니다.

### 프로젝트 1 — 자동화 도구 비교 구현

| 구분 | 내용 |
| --- | --- |
| 워크플로우 | Google Form(OX 퀴즈) 제출 → 채점 → 점수 기준 조건 분기 → Google Sheets 기록 + Gmail 발송 |
| 사용 도구 | Make, Zapier |
| Trigger | Google Forms — 응답 감지 |
| 조건 분기 | 점수 기준 Router(Make) / Filter(Zapier) |
| Action | Google Sheets 행 추가, Gmail 메일 발송 |

### 프로젝트 2 — 자유 주제 자동화

| 구분 | 내용 |
| --- | --- |
| 반복 업무 | Facebook 페이지 게시물 발행 및 발행 이력 기록 |
| 선정 도구 | Make |
| 선정 이유 | 시각적 분기 설계가 직관적이고 무료 Ops 범위 내 구현 가능 |
| 흐름 | Trigger → 콘텐츠 처리 → 조건 분기 → Facebook Pages 게시 + Google Docs 로깅 |

---

## 제출 문서

| 문서 | 내용 | 바로가기 |
| --- | --- | --- |
| `01_tool_comparison.md` | 프로젝트 1 — Make vs Zapier 동일 워크플로우 구현 · 비교 항목 5종 이상 · 장단점 · 적합 상황 의견 | [01_tool_comparison.md](./01_tool_comparison.md) |
| `02_custom_workflow.md` | 프로젝트 2 — 반복 업무 정의 · 도구 선정 이유 · 워크플로우 설계 · 실행 검증 | [02_custom_workflow.md](./02_custom_workflow.md) |
| `screenshots/` | 워크플로우 구성 화면 · 실행 결과 화면 캡처 (민감정보 마스킹 처리) | [screenshots](./screenshots) |

---

## 평가 기준 대응
>>>>>>> bc2fe14 (flowchart)

### 공통 요구 사항

| 평가 항목 | 대응 문서 | 해당 절 |
| --- | --- | --- |
<<<<<<< HEAD
| 실제 동작하는 워크플로우 구현 | [01_tool_comparison.md](./01_tool_comparison.md)| 실행 결과 |
| Trigger 1개 이상 포함 | [01_tool_comparison.md](./01_tool_comparison.md)| 워크플로우 구성 |
| Action 2개 이상 포함 | [01_tool_comparison.md](./01_tool_comparison.md)| 워크플로우 구성 |
| 조건 분기(Filter/Router) 1개 이상 포함 | [01_tool_comparison.md](./01_tool_comparison.md)| 조건 분기 설계 |
| 각 분기 경로 1회 이상 실행 결과 확인 | [01_tool_comparison.md](./01_tool_comparison.md) | 실행 결과 — 분기별 로그 |

=======
| 실제 동작하는 워크플로우 구현 | [01_tool_comparison.md](./01_tool_comparison.md), [02_custom_workflow.md](./02_custom_workflow.md) | 실행 결과 |
| Trigger 1개 이상 포함 | [01_tool_comparison.md](./01_tool_comparison.md), [02_custom_workflow.md](./02_custom_workflow.md) | 워크플로우 구성 |
| Action 2개 이상 포함 | [01_tool_comparison.md](./01_tool_comparison.md), [02_custom_workflow.md](./02_custom_workflow.md) | 워크플로우 구성 |
| 조건 분기(Filter/Router) 1개 이상 포함 | [01_tool_comparison.md](./01_tool_comparison.md), [02_custom_workflow.md](./02_custom_workflow.md) | 조건 분기 설계 |
| 각 분기 경로 1회 이상 실행 결과 확인 | [01_tool_comparison.md](./01_tool_comparison.md), [02_custom_workflow.md](./02_custom_workflow.md) | 실행 결과 — 분기별 로그 |

### 프로젝트 1
>>>>>>> bc2fe14 (flowchart)

| 평가 항목 | 대응 문서 | 해당 절 |
| --- | --- | --- |
| 서로 다른 2개 이상 자동화 도구 사용 | [01_tool_comparison.md](./01_tool_comparison.md) | 사용 도구 |
| 동일한 워크플로우 구조로 구현 | [01_tool_comparison.md](./01_tool_comparison.md) | 워크플로우 정의 |
| 도구별 워크플로우 구성 화면 캡처 | [01_tool_comparison.md](./01_tool_comparison.md) | Make 구현 · Zapier 구현 |
| 실행 결과 화면 캡처 | [01_tool_comparison.md](./01_tool_comparison.md) | 실행 결과 |
| 사용한 도구 이름 | [01_tool_comparison.md](./01_tool_comparison.md) | 사용 도구 |
| 구현 과정 요약 | [01_tool_comparison.md](./01_tool_comparison.md) | 구현 과정 |
| 비교 항목 5개 이상 | [01_tool_comparison.md](./01_tool_comparison.md) | 비교표 |
| 각 도구의 장단점 정리 | [01_tool_comparison.md](./01_tool_comparison.md) | 장단점 |
| 어떤 상황에서 적합한지 의견 | [01_tool_comparison.md](./01_tool_comparison.md) | 도구 선택 가이드 |

### 프로젝트 2

| 평가 항목 | 대응 문서 | 해당 절 |
| --- | --- | --- |
<<<<<<< HEAD
| 실제 동작하는 워크플로우 구현 | [01_tool_comparison.md](./01_tool_comparison.md)| 실행 결과 |
| Trigger 1개 이상 포함 | [01_tool_comparison.md](./01_tool_comparison.md)| 워크플로우 구성 |
| Action 2개 이상 포함 | [01_tool_comparison.md](./01_tool_comparison.md)| 워크플로우 구성 |
| 조건 분기(Filter/Router) 1개 이상 포함 | [01_tool_comparison.md](./01_tool_comparison.md)| 조건 분기 설계 |
| 각 분기 경로 1회 이상 실행 결과 확인 | [01_tool_comparison.md](./01_tool_comparison.md) | 실행 결과 — 분기별 로그 |

| 평가 항목 | 대응 문서 | 해당 절 |
| --- | --- | --- |
=======
>>>>>>> bc2fe14 (flowchart)
| 자동화할 반복 업무 1개 정의 | [02_custom_workflow.md](./02_custom_workflow.md) | 반복 업무 정의 |
| 도구 1개 선정 및 선정 이유 | [02_custom_workflow.md](./02_custom_workflow.md) | 도구 선정 |
| 워크플로우 설계 문서(설명 또는 다이어그램) | [02_custom_workflow.md](./02_custom_workflow.md) | 워크플로우 설계 |
| 자동 실행 구조 구현 | [02_custom_workflow.md](./02_custom_workflow.md) | 워크플로우 설계 — Trigger |
| 워크플로우 흐름 설명 | [02_custom_workflow.md](./02_custom_workflow.md) | 워크플로우 흐름 |
| 구현 화면 캡처 | [02_custom_workflow.md](./02_custom_workflow.md) | 구현 화면 |
| 실행 결과 화면 캡처 | [02_custom_workflow.md](./02_custom_workflow.md) | 실행 결과 |

### 보너스

| 평가 항목 | 대응 문서 | 해당 절 |
| --- | --- | --- |
<<<<<<< HEAD
| 보너스 1 — AI 연동 Action 추가 | (해당 시 기재) | — |
| 보너스 2 — 실패 알림 및 재시도/대체 경로 | (해당 시 기재) | — |

---
=======
| 보너스 1 — AI 연동 Action 추가 | ./project1/compareReport.md |  |
---

## 제약 사항 준수

| 제약 항목 | 준수 내용 | 근거 위치 |
| --- | --- | --- |
| 자동화 도구 2개 이상 직접 사용 | Make, Zapier 양쪽 직접 구현 | [01_tool_comparison.md](./01_tool_comparison.md) |
| 두 프로젝트 모두 실제 동작하는 워크플로우 | 실행 로그/결과 화면 첨부 | 각 문서 실행 결과 절 |
| 프로젝트 2는 Trigger 발생 시 자동 실행 | 수동 실행 아님 — Trigger 기반 자동 실행 | [02_custom_workflow.md](./02_custom_workflow.md) 워크플로우 설계 |
| API Key · 토큰 · 비밀번호 미노출 | 스크린샷 내 자격증명 영역 마스킹(***) | [screenshots](./screenshots) |
| 계정 이메일 일부 가림 처리 | 메일 주소 부분 마스킹 | [screenshots](./screenshots) |
| 무료 플랜 범위 내 구현 | Make 무료 Ops · Zapier 무료 Tasks 범위 내 | [01_tool_comparison.md](./01_tool_comparison.md) 비교표 — 무료 플랜 |
| 유료 기능 사용 시 불가피 사유 + 무료 대안 명시 | (해당 시 기재 / 미사용 시 "전 구간 무료 플랜") | [01_tool_comparison.md](./01_tool_comparison.md) |

---

## 개념 정리

| 개념 | 설명 |
| --- | --- |
| Trigger | 워크플로우 실행을 시작시키는 이벤트 |
| Action | Trigger 이후 수행되는 처리 동작 |
| Filter | 조건을 만족하는 경우에만 이후 단계를 통과시키는 단일 경로 게이트 |
| Router | 조건에 따라 서로 다른 경로로 분기시키는 다중 경로 구조 |
>>>>>>> bc2fe14 (flowchart)
