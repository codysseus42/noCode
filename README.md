
# 노코드 자동화 — 도구 비교 구현 및 자유 주제 파이프라인

## 프로젝트 개요

동일한 워크플로우를 Make와 n8n 두 도구로 구현해 비교 분석하고(프로젝트 1), 별도의 반복 업무를 자동화하는 파이프라인을 설계·구현한(프로젝트 2) 프로젝트입니다.

### 프로젝트 1 — 자동화 도구 비교 구현

| 구분 | 내용 |
| --- | --- |
| 워크플로우 | Google Form(OX 퀴즈) 제출 → 채점 → 점수 기준 조건 분기 → Google Sheets 기록 + Gmail 발송 |
| 사용 도구 | Make, n8n |
| Trigger | Schedule Google Spread Sheet — 응답 감지 |
| 조건 분기 | 점수 기준 Router(Make) / Filter(n8n) |
| Action | Google Sheets 행 추가, Gmail 메일 발송 |

### 프로젝트 2 — 웹훅 트리거와 rssFeed를 활용한 게시물 분류 요약 저장

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
| `compareReport.md` | 프로젝트 1 — Make vs n8n 동일 워크플로우 구현 · 비교 항목 5종 이상 · 장단점 · 적합 상황 의견 | [compareReport.md](./project1/compareReport.md) |
| `rss.md ` | 프로젝트 2 — 반복 업무 정의 · 도구 선정 이유 · 워크플로우 설계 · 실행 검증 | [rss.md](./projecrt2/rss.md.md) |

---

## 평가 기준 대응

### 공통 요구 사항

| 평가 항목 |
| --- |
| 실제 동작하는 워크플로우 구현 
| Trigger 1개 이상 포함  |
| Action 2개 이상 포함  |
| 조건 분기(Filter/Router) 1개 이상 포함 |
| 각 분기 경로 1회 이상 실행 결과 확인  |

### 프로젝트 1

| 평가 항목 | 대응 문서 | 해당 절 |
| --- | --- | --- |
| 실제 동작하는 워크플로우 구현 | [compareReport.md](./project1/compareReport.md)| [실행 결과]((./project1/compareReport.md#실행결과)) |
| Trigger 1개 이상 포함 |[compareReport.md](./project1/compareReport.md)| [워크플로우](./project1/compareReport.md#워크플로우) |
| Action 2개 이상 포함 | [compareReport.md](./project1/compareReport.md)| [워크플로우](./project1/compareReport.md#워크플로우) |
| 조건 분기(Filter/Router) 1개 이상 포함 | [compareReport.md](./project1/compareReport.md)| [워크플로우](./project1/compareReport.md#워크플로우)  |
| 각 분기 경로 1회 이상 실행 결과 확인 | [compareReport.md](./project1/compareReport.md)| [n8n 부터]((./project1/compareReport.md#n8n) |

| 평가 항목 | 대응 문서 | 해당 절 |
| --- | --- | --- |
| 서로 다른 2개 이상 자동화 도구 사용 |[compareReport.md](./project1/compareReport.md)| [사용 도구](./project1/compareReport.md#자동화-도구-구현make-vs-n8n-과정-요약)  |
| 사용한 도구 이름 | [01_tool_comparison.md](./01_tool_comparison.md) | [사용 도구](./project1/compareReport.md#자동화-도구-구현make-vs-n8n-과정-요약) |
| 동일한 워크플로우 구조로 구현 | [compareReport.md](./project1/compareReport.md)| [워크플로우](./project1/compareReport.md#워크플로우) |
| 도구별 워크플로우 구성 화면 캡처 | [compareReport.md](./project1/compareReport.md) |[n8n 구현 부터](./project1/compareReport.md#n8n-구현) |
| 구현 과정 요약 | [compareReport.md](./project1/compareReport.md) | [n8n구현과정요약 부터](./project1/compareReport.md#n8n구현과정요약) |
| 실행 결과 화면 캡처 | [compareReport.md](./project1/compareReport.md) |  [n8n 부터](./project1/compareReport.md#n8n)|
| 비교 항목 5개 이상 | [compareReport.md](./project1/compareReport.md) | [비교항목](./project1/compareReport.md#비교항목)|
| 각 도구의 장단점 정리 | [compareReport.md](./project1/compareReport.md) |[도구 장단점](./project1/compareReport.md#도구-장단점)|
| 어떤 상황에서 적합한지 의견 | [compareReport.md](./project1/compareReport.md) | [상황별 적합도 의견](./project1/compareReport.md#상황별-적합도-의견) |
|보너스1-워크플로우에 생성형 AI 를 Action 으로 추가해 텍스트를 자동 생성한다.| [compareReport.md](./project1/compareReport.md) | [과제 1Bonus](./project1/compareReport.md#Bonus)|

### 프로젝트 2

| 평가 항목 | 대응 문서 | 해당 절 |
| --- | --- | --- |
| 실제 동작하는 워크플로우 구현 |  [rss.md](./projecrt2/rss.md)| [실행 결과] |
| Trigger 1개 이상 포함 |  [rss.md](./projecrt2/rss.md)| [[워크플로우 설계](./project2/rss.md#워크플로우-설계)] |
| Action 2개 이상 포함 |  [rss.md](./projecrt2/rss.md)| [[워크플로우 설계](./project2/rss.md#워크플로우-설계)] |
| 조건 분기(Filter/Router) 1개 이상 포함 |  [rss.md](./projecrt2/rss.md)| [조건 분기 설계] |
| 각 분기 경로 1회 이상 실행 결과 확인 |  [rss.md](./projecrt2/rss.md) | [실행 결과 — 분기별 로그] |

| 평가 항목 | 대응 문서 | 해당 절 |
| --- | --- | --- |
| 자동화할 반복 업무 1개 정의 | [rss.md](./projecrt2/rss.md) | [0반복 업무 정의](./project2/rss.md#업무-정의) |
| 도구 1개 선정 및 선정 이유 | [rss.md](./projecrt2/rss.md) | [도구와 선정 이유](./project2/rss.md#도구와-선정-이유) |
| 워크플로우 설계 문서(설명 또는 다이어그램) | [rss.md](./projecrt2/rss.md) | [워크플로우 설계](./project2/rss.md#워크플로우-설계) |
| 자동 실행 구조 구현 | [rss.md](./projecrt2/rss.md) | 워크플로우 설계 — Trigger |
| 워크플로우 흐름 설명 | [rss.md](./projecrt2/rss.md) | 워크플로우 흐름 |
| 구현 화면 캡처 | [rss.md](./projecrt2/rss.md) | 구현 화면 |
| 실행 결과 화면 캡처 | [rss.md](./projecrt2/rss.md) | 실행 결과 |
|보너스1-워크플로우에 생성형 AI 를 Action 으로 추가해 텍스트를 자동 생성한다.| [rss.md](./projecrt2/rss.md)| [과제2Bonus](./project2/rss.md#Bonus)|

### 보너스
보너스 1 — AI 연동 Action 추가
| 평가 항목 | 대응 문서 | 해당 절 |
| --- | --- | --- |
|워크플로우에 생성형 AI 를 Action 으로 추가해 텍스트를 자동 생성한다.|[compareReport.md](./project1/compareReport.md) , [rss.md](./projecrt2/rss.md)| [과제 1Bonus](./project1/compareReport.md#Bonus),[과제2Bonus](./project2/rss.md#Bonus)|
## 제약 사항 준수

| 제약 항목 | 준수 내용 | 
| --- | --- |
| 자동화 도구 2개 이상 직접 사용 | Make, n8n 양쪽 직접 구현 | 
| 두 프로젝트 모두 실제 동작하는 워크플로우 | 실행 로그/결과 화면 첨부 |
| 프로젝트 2는 Trigger 발생 시 자동 실행 | 웹훅 입력과 rss피드를 10분 주기로 읽어서 실행하도록 하였습니다. | 
| API Key · 토큰 · 비밀번호 미노출 | 스크린샷 내 해당부분 가림|
| 계정 이메일 일부 가림 처리 | 메일 주소 부분 가림 |
| 무료 플랜 범위 내 구현 | Make 무료 1000토큰 이내, n8n 2간의 무료 체험기간 이용 이후 필요하면 Docker를 통해서 계속 자체호스팅 가능| 
| 유료 기능 사용 시 불가피 사유 + 무료 대안 명시 |두 프로젝트에서 제미나이는 사용 편의성과 로그 관찰을 위해서 자체 API를 구입하여 사용하였습니다. 제미나이는 구글 ai 스튜디오를 통해 처음 가입자에게 무료 크레딧을 제공하고 oai 등 다른 llm의 개발자 시스템에서도 무료 사용가능한 api크레딧을 제공하는 것으로 알고 있습니다.|

## 개념 정리

| 개념 | 설명 |
| --- | --- |
| Trigger | 워크플로우 실행을 시작시키는 이벤트 |
| Action | Trigger 이후 수행되는 처리 동작 |
| Filter | 조건을 만족하는 경우에만 이후 단계를 통과시키는 단일 경로 게이트 |
| Router | 조건에 따라 서로 다른 경로로 분기시키는 다중 경로 구조 |
