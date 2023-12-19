---
title: ChatGPT 데이터 재구성(요약, 재작성, 간소화)
date: 2023-12-19
categories: [blog, prompt]
tags: [chatgpt]
---
## 내용이 많은 텍스트 요약하기

예: 로켓 과학 기사의 이 구절을 요약해줘.

- 요약: 정보에 대한 간결하고 간결한 요약을 제공하고 핵심 사항과 주요 아이디어를 강조합니다.

- Paraphrasing((알기 쉽게) 바꾸어 쓰다)해줘: 동일한 의미와 핵심 정보를 유지하면서 다른 형태로 정보를 재작성합니다.

- 불렛 형식으로 요약해줘: 주요 테이크아웃 또는  불렛 형식 목록에 정보를 압축합니다.

- 원문에서 "중요한 10 키워드, 중요한 날짜, 중요한 사람들의 목록"을 추출해줘 : 정보에서 가장 중요한 용어와 문구를 파악하여 나열합니다.

- 원본 텍스트를 단순화해줘: 정보를 더 단순하고 이해하기 쉬운 언어로 표현합니다.

- 시각적 표현으로 만들어줘: 다이어그램, 차트 또는 그래프와 같은 정보의 시각적 표현을 생성합니다.

- 원문 텍스트에 도움이 되는 비유를 들고 그것과 비교해줘: 정보를 비슷한 개념이나 아이디어와 비교하여 이해하기 쉽도록 합니다.

- 중요한 정보 강조: 정보의 가장 중요한 부분을 강조합니다.

- 지금 배우고 있는 것의 배경 지식(컨텍스트)을 제공해줘: 정보를 보다 쉽게 이해할 수 있도록 추가 정보나 배경을 제공합니다.

- 보너스: 요약을 스탠드업 코미디 루틴으로 바꿔줘.

## 특정 청자에 맞게 설명해줘.

청자의 배경 지식이 다르다고 가정하기 때문에 응답에 큰 영향을 끼침.

> 중력에 대해 설명해줘.

- 초등학교 5학년에게 설명하듯이 

> 컴퓨팅 성능의 증가에 대해 다양한 산업 분야 종사자 대상으로 설명해줘

- 소프트웨어 개발자 대상으로
- 마케팅 담당자 대상으로


## 언어번역해줘

이제 구글 번역기보다 chatGPT가 번역을 더 잘해요!

> 이 텍스트를 번역해줘, 1. 영어, 2 이탈리아어, 3. 프랑스어

> 이 글의 요점을 네가지 불렛 포인트로 요약해줘.

> 이 글을 원어민이 적은 것처럼 바꿔줘. (덜 로봇같은 글을 만들어주는 파라미터)

## 동영상 요약하기 

YouTube Summary with ChatGPT & Claude 확장 프로그램을 사용하면 된다.

<iframe width="100%" height="400" src="https://chromewebstore.google.com/detail/youtube-summary-with-chat/nmmicjeknamkfloonkhhcjmomieiodli" allowfullscreen="allowfullscreen" allowpaymentrequest frameborder="0"></iframe>

[https://chromewebstore.google.com/detail/youtube-summary-with-chat/nmmicjeknamkfloonkhhcjmomieiodli](https://chromewebstore.google.com/detail/youtube-summary-with-chat/nmmicjeknamkfloonkhhcjmomieiodli)

## 문서 교정하기

> 이 텍스트의 모든 문법 오류 삭제 해줘.

> course 라는 단어를 찾아서 class로 바꿔주고, 숫자 4를 모두 6으로 바꿔줘.

> 이 텍스트에서 복잡한 용어를 이해하기 쉬운 용어로 바꿔줘. 일반인이 읽기 쉽도록.

## 데이터 재구성 : 기본

이 데이터를 

> 알파벳순으로 정렬해줘
>
> 시간순으로 정리해줘
>
> 표로 정리해줘
>
> 지리적 위치를 나타내는 열을 추가해줘

표 형식으로 만들면 다른 에디터에 넣어도 형식이 그대로 유지됩니다.


## 데이터 재구성 : 이름 찾기

> 다음 텍스트에서 이름, 날짜, 위치를 추출해 줘"


## 데이터 재구성 : 고급 재구성 및 텍스트 분류

ChatGPT 로 텍스트 데이터를 내용에 따라 여러 클래스나 카테고리로 분류할 수 있어요.

**텍스트 분류(Text Classification)**는, 텍스트 데이터에 그 내용에 따라 미리 정의된 카테고리나 레이블을 할당하는 작업입니다.

> 사건의 종류에 따라 표로 정리해줘
>
> 중요도에 따라 표로 정리해줘


## 데이터 재구성 : 차트 만들기

강사쌤 : 우연히 발견한건데 짱이에요!

1단계: 데이터 테이블을 복사합니다

2단계: ChatGPT에 데이터 테이블을 넣고, 여러분이 원하는 차트 유형으로 데이터를 표시하는 웹사이트를 만들어 달라고 요청하세요. 그 데이터를 해석해 달라고 요청할 수도 있어요.

3단계: 완료될 때까지 기다렸다가 코드를 복사합니다.

4단계: 텍스트 편집기를 열고 코드를 붙여넣은 다음, 확장자 .HTML로 파일을 저장합니다


## 데이터 재구성 : 문제 만들기

에세이 문제, 객관식 문제, 빈칸 채우기 문제, 참 거짓 문제 등.

수작업으로 만드려면 엄청나게 많은 시간이 걸립니다.

> 아래 정보에 관한 객관식 문제 10개를 만들어줘
>
> 이 문제들에 대한 정답을 만들어줘.

## 데이터 재구성 : 웹 검색용 확장프로그램


**WebChatGPT: 인터넷 액세스 가능한 ChatGPT**

크롬 익스텐션입니다. 2021년도 이후 관련 질문을 하고 싶다면 이 익스텐션을 깔면 됩니다. 

웹검색이 목적일땐 여기서 주는 기본 프롬프트가 가장 잘 작동합니다.

[https://chromewebstore.google.com/detail/webchatgpt-%EC%9D%B8%ED%84%B0%EB%84%B7-%EC%95%A1%EC%84%B8%EC%8A%A4-%EA%B0%80%EB%8A%A5%ED%95%9C-ch/lpfemeioodjbpieminkklglpmhlngfcn?hl=ko](https://chromewebstore.google.com/detail/webchatgpt-%EC%9D%B8%ED%84%B0%EB%84%B7-%EC%95%A1%EC%84%B8%EC%8A%A4-%EA%B0%80%EB%8A%A5%ED%95%9C-ch/lpfemeioodjbpieminkklglpmhlngfcn?hl=ko)
