# 자유 주제 자동화 설계 및 구현: rss피드를 통한 뉴스 요약

## 업무 정의
웹폼(Tally)을 통해서 게시물이 접수되면 Webhook으로 호출하거나 정각부터 매 10분마다 블로그(Blogger)에 접수된 게시물을 RSSFEED를 통해서 조회하는 2가지 Trigger를 가지고
두 소스에서 접수된 게시물들에 규칙에 따른 guid를 검사하고 내용이 없으면 오류처리 내용이 있으면 고양이에 대한 내용인지, 긍정,부정,중립중 어떤 내용인지 분류하고 게시물을 요약한 결과물을 json으로 받고, 고양이 관련 내용인지 아닌지에 따라 notion 데이터베이스 페이지에 저장한다.

## 도구와 선정 이유
선정 도구:n8n

많은 시행착오가 필요했기 때문에 시행착오를 하여도 재시도 할 수 있는 도구가 필요했고, 업무특성상 다건의 데이터처리가 편하게 연동되는 도구를 선택하고자 하였습니다.
현재 무료사용 기간이어서 다른 대안(make)보다 많은 사용양을 확보할 수 있고, 이후 기간에 반복 실행하여도 자체 호스팅을 통해 실행하면 비용을 아낄 수 있으며, 여러건의 입력 값에 대해 묶음 처리가 수월하여 선정하였습니다.

## 구현 화면 캡처

![workflow](./image/projectbworkflow.png)

## 워크플로우 설계
Trigger에 대해서는  [Trigger](./project2/rss.md#Trigger)를 봐주세요.
    
```mermaid
graph TD

       A["Trigger 작동(Rss Webhook"] --> B["Action 1 Get many database pages notion에서 전체 데이터 베이스조회"] --> C["Action2 Aggregate 필터에서 중복 조회를 위해서 해당 값을 취합"] --> D["Acrion3 구조화 GUID 생성"]  --> E["Acrion4 Filter 필터에서 중복 조회"]  --> F
       A1"Trigger 작동(Schedule Trigger or Webhook"] --> B1["Action 1 Get many database pages notion에서 전체 데이터 베이스조회"] --> C1["Action2 Aggregate 필터에서 중복 조회를 위해서 해당 값을 취합"] --> D1["Action3 구조화 GUID 생성"] --> E1["Acrion4 Filter 필터에서 중복 조회"] --> F["Action5 양쪽 경로에서 온 값들을 취합"] - -> G{"if 조건분기 게시물에 내용이 있는지에 따라 분기"}
       G --> |내용이 있음| I["Action 6 Message a model 입력된 내용을 분석 해서 형식에 맞게 출력"] --> J["Action 7 입력전에 구조화"] ---> K{"f 조건분기2"} 
        K -->  |고양이 내용임| L["Action 8 고양이 내용으로 (sentiment, status=complete,summary 포함하여) NotionDB에 저장"] 
        K --> |고양이 내용 아님| M["Action9 고양이 아닌 내용으로 (sentiment, status=noCat,summary 비포함하여) Norion DB에 저장"]
      G --> |내용이 없음| N["Action 10 Stop and Error Error Message로 No content Error전달"]  --> P["Error Trigger 에러 발생시 작동"] --> P["Action 11 Discord로 Error Message Work Flow전송"]
```

### Trigger
<details>
<summary>Trigger발생시 자동 실행 </summary>

#### 스케줄러

![schedule](./image/scheduleTrigger.png)

정각부터 10,20,30..50 다음정각까지 시간에 맞춰 작동한다.

![tallywebhook](./image/tallywebhook.png)

#### 웹훅
![webhook](./image/webhookTrigger.png)

웹폼으로 접수되면 설정해놓은 주소로 요청하고 요청받으면  작동한다.


</details>

### Aggregator
<details>
<summary>Aggregator </summary>

![aggregator](./image/aggregator.png)

notion에서 조회한 guid값들을 모은다.

</details>

### Edit FIelds 
<details>
<summary>구조화 GUID 생성 </summary>

![rssguid](./image/rssguid.png)

![tallyguid](./image/tallyguid.png)

기존 GUID들을  석이지 않게 앞에 tally,rss값을 붙여준다.(혹시 양쪽 소스의 guid가 겹치지 않게)

</details>

### Filter
<details>
<summary>중복검사 </summary>

![filter](./image/filter.png)

Aggregator에서 조회한 guid들과 위의 EditField에서 만든 guid를 대조한다.

</details>

### if조건 분기1
<details>
<summary>내용 유무 </summary>

![if1](./image/if1.png)

내용이 없으면 내용 없음 에러로
내용이 있으면 AI Action으로

</details>

### if조건 분기2 
<details>
<summary>고양이 내용인지 여부</summary>

![if2](./image/if2.png)

![ifCat](./image/ifcat.png)

![ifnoCat](./image/ifnocat.png)

</details>


## 실행 결과 화면 캡처 

<details>
<summary>입력값</summary>

웹폼(Tally)
![inputtally](./image/prbsubmission.png)

각각 고양이 부정적 내용과 내용없는 오류 입력입니다.

블로그(Blogger)
![inputblog](./image/prbblog.png)

고양이에 대한 중립적, 긍정적 게시물과 고양이와 관련 없는 송사리 관련 게시물입니다.

</details>

![log](./image/prbloglist.png)

![flowwebhook](./image/prbflowwebhook.png)

<details>
<summary>webhook 결과</summary>

![notionnegative](./image/prbnotionnegative.png)

</details>

![flowrss](./image/prbflowrss.png)

<details>
<summary>rss 결과</summary>

![notionnegative](./image/prbnotionpositive.png)

![notionneutral](./image/prbnotionneutral.png)

</details>

![flownocat](./image/prbflownocat.png)

<details>
<summary>고양이 아님</summary>

![notionnocat](./image/prbnotionnocat.png)

</details>

![flowerr](./image/prbflowerror.png)
<details>
<summary>내용 없음 오류</summary>

![discorderr](./image/discorderr.png)

</details>

## Bonus

![geminilog](./image/prbgemini.png)

```
You are a content analyzer.
Read the following title and content, then follow these steps:

1. Check if it is about a cat.
   - If NOT about a cat, set isCat to "N", 
     leave sentiment and summary empty, and stop.
   - If about a cat, set isCat to "Y" and continue.

2. Write a brief summary of the post (1-2 sentences).

3. Analyze the sentiment and choose ONE:
   'positive', 'negative', or 'neutral'.

Return ONLY valid JSON in this exact format:
{
  "isCat": "Y or N",
  "sentiment": "positive, negative, or neutral",
  "summary": "brief summary here"
}

title: {{ $('Merge').item.json.title }}
content: {{ $('Merge').item.json.content }}
```

![geminilog](./image/prbAINodew.png)


