# 비교 분석 보고서

## 자동화 도구 구현(Make vs n8n) 과정 요약
매시간 정각 15분 30분 45분 별로 입력된 ox퀴즈결과에 ai를 이용해서 주관식(편지)을 체점하고 주관식에 코멘트(답장)를 달아서 점수에 따라 다른 시트에 기록하고 결과 메일을 전송하는 워크플로우 입니다. 

### 워크플로우:

```mermaid
graph LR
       A["1 Google Form제출"] --> B["2 구글 스프레드 시트 파일(Simple OX Quiz-Form Repsponses)에 응답기록(구글 폼 자체기능)"] --> C["3 Trigger Cron 식으로 매 정각부터 15분 주기로 자동 실행(0,15,30,45 다음 정각)"] --> C1["4 Acrion1 Simple OX Quiz-Form Repsponses에서 새로 들어온 입력 읽어들임"] --> D["5 Acrion2 제미나이 연동 주관식 체점 기존점수에 주관식 점수(교사에게 보내는 편지 체점 0 or 1)를 더 함"] --> E["6 Acrion3 제미나이 주관식 내용에 코멘트(교사의 답장)  보너스1"] --> F{" 7 조건/분기(Router) 5에서 채점한 점수에 따라 분기"}
       F --> |3 점이상| G["8 Action 4 구글 스프레드시트 record - pass record 시트에 점수와 답변내용 이메일주소, 제출시간,현재시간,isNew Column y로 기록"] --> H["9 Action 5 구글 프레드시트 pass record sheet에서 isNew  값이 y인 줄들만 읽어들임 후에 메일 전송시 사용"] --> I["10 Action 6 스프레드 시트 record - pass record 에서 9에서 읽어들인 행번호의 isNew 값 N 으로 업데이트"] --> J["11 Action 7  9에서 읽어들인 값들로 메일전송"] 
      F --> |3 점 미만| K["12 Action 4 구글 스프레드시트 record - fail record 시트에 점수와 답변내용 이메일주소, 제출시간,현재시간,isNew Column y로 기록"] --> L["13 Action 5 구글 프레드시트  record - fail record에서 isNew  값이 y인 줄들만 읽어들임 후에 메일 전송시 사용"] --> M["14 Action 6 스프레드 시트에서 13에서 읽어들인 행번호의 isNew 값N 으로 업데이트"] --> N["15 Action 7 13에서 읽어들인 값들로 메일전송"]
```
분기 이후 Action은 한쪽 경로로만 넘버링하였습니다.
### n8n 구현

#### n8n 워크 플로우 구현 화면 캡쳐 
![n8nflow](./image/n8nworkflow.png)

#### n8n구현과정요약
구조가 같기 떄문에 한분기로 요약합니다.

    -Trigger and Action1: Google Sheets Trigger
    -Action2:Analyze document(주관식 평가후 합산)
    -Action3: Analyze document(주관식 답변(편지에 대한 답장 생성)
    -if(true/false) Action2의 점수 3점이상 합격으로/3점미만 불합격으로)
    - Action4:Append sheet - 합격자 불합격자들을 각각 다른 sheet(pass/fail)에 추가 이때 isNew컬럼 값은 y로 입력
    - Action5: Get row(s) in sheet - isNew Column이 y인 컬럼들을 모음
    - Action6: Update row in sheet - 전에 읽어들인 isNew Column의 값을 N으로 바꿈
    -Action7: Send message -Action 5에서 읽어들인 내용으로 메일을 전송함


### [Make 구현]

#### Make 워크 플로우 구현 화면 캡쳐 1(초기버전)
![makeflow](./image/makeflow1.png)

초기 구현 form에서 직접 읽어 들이는 기능을 사용해봤지만 n8n에서는 지원이 되지 않아 변경하였습니다.

#### Make 워크 플로우 구현 화면 캡쳐 2(실사용 버전)
![makeflow](./image/makeflow2.png)

n8n과 일치하게  Form spread sheet에 맞게 변경 되었습니다.

#### make구현과정요약

구조가 같기 떄문에 한분기로 요약합니다.

    -Trigger and Action1: Google Sheets-watch new rows
    -Action2:Google Gemini AI-Generate a Response(주관식 평가후 합산)
    -Action3:Google Gemini AI-Generate a Response(주관식 답변(편지에 대한 답장 생성)
    -Router(pass/fail) Action2의 점수 3점이상 합격으로/3점미만 불합격으로)
    - Aggregator 하나씩 처리된 답변들을 모음
    - Filter Aggregator의 결과가 0보다 크면 통과
    - Action4: Google Sheets-Bulk AddRows - 합격자 불합격자들을 각각 다른 sheet(pass/fail)에 추가 이때 isNew컬럼 값은 y로 입력
    - Action5: Google Sheets- Search Rows - isNew Column이 y인 컬럼들을 모음
    - Action6: Google Sheets-Update a Row - 전에 읽어들인 isNew Column의 값을 N으로 바꿈
    -Action7: Gmail-Send a mail -Action 5에서 읽어들인 내용으로 메일을 전송함

#### Make 특이사항

01 Aggregator

![Aggregator1](./image/104Aggregator.png)

Gemini 채점, 답장 생성 답변이후 한번에 실행하기 위해 Aggregator 로 모아주어야 합니다.

02 Filter

![Aggregator2](./image/103AggregatorEmpty.png)

![Filter](./image/102FilterInAction.png)

Aggregator가 입력이 없는 경우도 취합하기 때문에 Filter가 필요합니다.

03 Cron 형

![Trigger](./image/102FilterInAction.png)

크론형 트리거로 작동 시키기 위해서 작동시간을 곧 있을 0,15,30,45 분중 하나로 맞춰주고 하루 종일 작동하게 맞춰 줌니다.


## 실행결과

<details>
<summary>  ### 입력전 </summary> 

![responseStartFrom](./image/responseStart.png)

![StratResponse](./image/01startResponse.png)

![beforepass](./image/150beforepass.png)

![beforefail](./image/151beforefail.png)

</details>

<details>
<summary>입력 </summary>

![input1](./image/05input1.png)

![input2](./image/06input2.png)

</details>

<details>
<summary>입력후 </summary>

![StratResponseForm](./image/n8nfirstForm.png)

![inputResponse1](./image/08intialinput2.png)

![inputResponse2](./image/09intitialinput3.png)

![StratResponse](./image/FirstExcel.png)

</details>

<details>
<summary>2회 </summary>

![SecondResponse](./image/130secondrunresponse.png)

![SecondResponseForm](./image/160secondrunresponse.png)

![SecondResponseForm](./image/131secondRunExcel.png)
</details>

### 공통실행결과

입력 시간대 조정문제로 timestamp시간에 차이가 생겼지만 시간 기록은 일관됨니다. 

#### 1회차 액셀

![FirstPass](./image/firstpass.png)

![failFirstlist](./image/Firstfail.png)

#### 2회차 액셀 

![allExcel](./image/allExcel.png)

![failsecondlist](./image/164secondFail.png)

#### 1회차(메일): 주관식 미응답(0점추가 합격), 주관식 응답(1점 추가 합격), 주관식 조롱(0점추가 불합격)


주관식 미응답(0점추가 합격)

![Accepted5list](./image/121gmai2list.png)

주관식 응답(1점 추가 합격)

![Accepted3list](./image/120gmail1list.png)

주관식 조롱(0점추가 불합격)

![fail2list](./image/122icloudlist.png)

둘을 한꺼번에 작동 시킨 결과 마지막에 n8n 작업 내용을 읽어 들여서 실패 메일을 1개 더 전송하였으나 단독으로 작동 시킬 경우 로직 상에는 문제가 없어서 해당 테스트 내용을 첨부 하였습니다.

#### 2회차(메일): 주관식 응답(1점추가) 불합격 된결과 입니다.

![failsecondlist](./image/139outlooklist.png)

### n8n

#### 워크플로우 실행 히스토리

![n8nHistList](./image/111n8nhistlist.png)

![makefirstRun](./image/112n8nhist1.png)

![makesecondRun](./image/113n8nhist2.png)

#### n8n 실행 결과 화면 캡처(메일)

AI 답변의 생성 Action은  [Bonus](./project1/compareReport.md#Bonus)를 참고해주세요.

1. gmail1

![n8ngmail1](./image/126n8ngmail2.png)

2. gmail2

![n8ngmail2](./image/125n8ngmail1.png)

3. cloud mail

![n8nicloud](./image/171n8nicloud.png)

4. outlook(2회차)

![n8noutlook](./image/174n8noutlook.png)

### Make

#### 워크플로우 실행 히스토리

AI 답변의 생성 Action은  [Bonus](./project1/compareReport.md#Bonus)를 참고해주세요.

![makeHistList](./image/114makehistlist.png)

![makefirstRun](./image/115makehist1.png)

![makesecondRun](./image/116makehist2.png)

#### Make 실행 결과 화면 캡처(메일)

1. gmail1

![makegmail1](./image/127makegmail1.png)

2. gmail2

![makegmail1](./image/128makegmail2
.png)

3. cloud mail

![makeicloud1](./image/172makeicloud1.png)

![makeicloud2](./image/173makeicloud2.png)

둘을 한꺼번에 작동 시킨 결과 마지막에 n8n 작업 내용을 읽어 들여서 실패 메일을 1개 더 전송하였으나 단독으로 작동 시킬 경우 로직 상에는 문제가 없어서 해당 테스트 내용을 첨부 하였습니다.

4. outlook(2회차)

![makeoutlook](./image/175makeoutlook.png)


## 비교항목
|비교항목 | Make | n8n | 
| --- | --- | --- |
| 1 UI/UX|  | | 
| 2 설정난이도 |동일한 기능에 한하여 조금더 직관적이고  | | 
| 3 연동 서비스 범위 | | | 
| 4 무료플랜 | 매달 1000 크레딧 (core요금제 기준 1크레딧당 $0.0009) |영구적인 무료플랜 제공하지 않으나 자체호스팅 가능 이 경우 셀프 호스팅 비용 만큼으로 줄일 수 있음| 
| 5 구현 기능 편의성(스케줄, 업무단위 분리) |  | | 
| 결론 | 일반인이 사용하고 1달안에 많은 실행을 반복할 필요가 없는 경우에는 조금더 연동성이 편리한 |  | 

## 도구 장단점

## 상황별 적합도 의견

## Bonus
 
 AI 연동 Action 추가
 
  ![geminiList](./image/geminiList.png) 
 
AI연동 Action에서는 모든 작업에서 제미나이의 최신 Flash(일반)버전(2026/08 현재: Flash 3.6)을 사용하였습니다.
 
 시험의 주관식 내용은 편지였으며 이에대한 답변을 제미나이를 통해서 생성하였습니다.
 초등교육 교사를 상정하고 학생의 주관식(편지에)
 친근한 답변을 달게 끔 프롬프팅하였습니다.
 일반적인 편지에는 진솔한 답변을
 욕설이나 공개적인 모욕에는 언어지도를(테스트에서 실행하지는 않았습니다.)
 비꼬는 편지나 비어있으면 유머러스하게 15자정도로 평문을 형성하게 하였습니다.
 
 체점에서도 제미나이 연동을 사용하였고
  채점은 아래 기준을 만족 하면 1점을 아니면 0점을 부여 하였습니다.
1. 10자이상
2. 욕설 없음
3.  비꼬거나 모욕하는 표현
 
 ### n8n
 
 답변
 
 ```
As a grade school teacher. you right back student's letter from heart. a friendly short answer. 
genuine answers to genuine letters.
when student use curse word or explicitly try to insult just give them a warning on their language and encourage to use proper language.  when student try clever insults or sarcasm or gave you empty answer, answer humorously. your answer will be less than 15 words.
just give plain sentece as result
Student's letter :
 {{ $('Google Sheets Trigger').item.json['write some letter from heart'] }}
 ```  

![n8nletter](./image/n8nletter.png)

 채점
 
  ```
 You are an objective grading assistant. Evaluate the following student answer based on these strict rules:

1. The answer must be MORE than 10 characters/letters long. that means if shorter or no letter then the point to written answer will be 0
 2. The answer must NOT contain any insulting curse words (e.g., F**k, sh*t).
3. The answer must NOT contain clever insults or sarcasm directed at the examiner. check carefully
GRADING RULE: If the written answer satisfies all 3 conditions and does not contain any of the prohibited content(rule2~3), you just consider it to be right answer I don't care whether it's gibberish, assign exactly 1 point. If it fails any condition, assign 0 points.
4. make sure the out put should be only final result. no words required just the final point

so your grade to the written answer will be the numeric score (either 1 or 0) and nothing else. and final result will be the integer that is your grading to written answer +{{ $json.Score }}

Student's written Answer to Grade:
 {{ $json['write some letter from heart'] }}
 ```
 
 ![n8nprompt2](./image/n8ngrade.png) 
 
 ### Make
 
 답변
 
시험의 주관식 내용은 편지였으며 이에대한 답변을 제미나이를 통해서 생성하였습니다.

채점 
 
 ```
 As a grade school teacher. you right back student's letter from heart. a friendly short answer. 
genuine answers to genuine letters.
when student use curse word or explicitly try to insult just give them a warning on their language and encourage to use proper language.  when student try clever insults or sarcasm or gave you empty answer, answer humorously. your answer will be less than 15 words.
just give plain sentece as result
Student's letter :
{{77.`8`}}
```

![makeprompt2](./image/makeresponseprompt2.png) 
  
![makeprompt1](./image/makeresponseprompt1.png) 

![makeletter](./image/makeletter.png)
 
 채점

make 모듈에서는 객관식 점수를 1/5단위로 받아와서 *5를 추가하였습니다.

 ```
You are an objective grading assistant. Evaluate the following student answer based on these strict rules:

1. The answer must be MORE than 10 characters/letters long. that means if shorter or no letter then the point to written answer will be 0
 2. The answer must NOT contain any insulting curse words (e.g., F**k, sh*t).
3. The answer must NOT contain clever insults or sarcasm directed at the examiner. check carefully
GRADING RULE: If the written answer satisfies all 3 conditions and does not contain any of the prohibited content(rule2~3), you just consider it to be right answer I don't care whether it's gibberish, assign exactly 1 point. If it fails any condition, assign 0 points.
4. make sure the out put should be only final result. no words required just the final point

so your grade to the written answer will be the numeric score (either 1 or 0) and nothing else. and final result will be the integer that is your grading to written answer + {{77.`1`}}*5

Student's written Answer to Grade:
{{77.`8`}}
 ```
 
![makeprompt1](./image/makeprompt1.png) 
  
![makeprompt2](./image/makeprompt2.png) 

![makegrade](./image/makegrade.png) 
 