# 자유 주제 자동화 설계 및 구현: rss피드를 통한 뉴스 요약

## 업무 정의
웹폼(Tally)을 통해서 게시물이 접수되면 Webhook으로 호출하거나 10분마다 블로그(Blogger)에 접수된 게시물을 RSSFEED를 통해서 조회하는 2가지 Trigger를 가지고
두 소스에서 접수된 게시물들에 규칙에 따른 guid를 검사하고 내용이 없으면 오류처리 내용이 있으면 고양이에 대한 내용인지, 긍정,부정,중립중 어떤 내용인지 분류하고 게시물을 요약한 결과물을 json으로 받고, 고양이 관련 내용인지 아닌지에 따라 notion 데이터베이스 페이지에 저장한다.

## 도구와 선정 이유
선정 도구:n8n

현재 무료사용 기간이어서 다른 대안(make)보다 많은 사용양을 확보할 수 있고, 이후 기간에 반복 실행하여도 자체 호스팅을 통해 실행하면 비용을 아낄 수 있으며, 여러건의 입력 값에 대해 묶음 처리가 수월하여 선정하였습니다.

## 구현 화면 캡처

![workflow](./image/projectbworkflow.png)

## 워크플로우 설계

### Trigger

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


