---
title: 가상 면접 사례로 배우는 대규모 시스템 설계 기초 - 개략적인 규모 추정
date: 2023-12-18
categories: [blog, system_design]
tags: [book]
---

시스템 설계 면접 시 시스템 용량, 성능 요구 사항을 개략적으로 추정해봐야 한다.

규모확장성을 표현하려면 기본적으로 3가지를 잘 이해해야 한다.

1. 2의 제곱수 
2. 응답지연 값 (Latency)
3. 가용성에 관계된 수치 

## 2의 제곱수 

데이터의 볼륨 단위를 2의 제곱수로 표현하는 법

아스키 문자 하나 : 1byte

| 2의 x 제곱 | 근사치 | 이름        | 축약형 |
| ---------- | ------ | ----------- | ------ |
| 10         | 천     | 1킬로바이트 | 1KB    |
| 20         | 백만   | 1메가바이트 | 1MB    |
| 30         | 십억   | 1기가바이트 | 1GB    |
| 40         | 십조   | 1테라바이트 | 1TB    |
| 50         | 천조   | 1페타바이트 | 1PB    |

## 응답지연 값 (Latency)

2010년도에 구글 프로그래머가 공개한 평범한 컴퓨터에서의 레이턴시 값으로 컴퓨터의 연산 속도를 짐작하게 해준다. (2010년도 값이라 참고만 하기)

<iframe 
  src="https://colin-scott.github.io/personal_website/research/interactive_latency.html"
  frameborder="0" allowfullscreen="true"
  scrolling="no" style="width : 80%; height : 30vh"></iframe>

   > 모든 프로그래머가 알아야 하는 응답 지연 값
   >
   > [https://colin-scott.github.io/personal_website/research/interactive_latency.html](https://colin-scott.github.io/personal_website/research/interactive_latency.html)

   - 메모리는 빨라졌지만 디스크는 아직도 느리다.
   - 디스크 탐색(seek) 는 가능한 피하라.
   - 단순한 압축 알고리즘은 빠르다.
   - 데이터를 인터넷으로 전송하기 전에 가능하면 압축하라.
   - 데이터센터는 보통 여러 지역에 분산되어 있고 센터들 간에 데이터를 주고 받는 데에는 시간이 걸린다.

## 가용성에 관계된 수치들

고가용성(High availability)은 **시스템이 중단없이 오랜 시간 동안 운영될 수 있는 능력**을 뜻한다.

가용성을 표현하는 방식은 % 단위로, 100% 는 단 한번도 중단된 적 없다는 뜻이다.

### SLA(Service Level Agreement)
SLA(서비스 단계 합의)는 서비스 제공자와 고객이 맺은 합의를 뜻한다. 서비스의 가용시간이 공식적으로 기술되어 있다.

aws, 구글, microsoft 같은 사업자는 99% 이상의 가용 시간을 제공한다.

> 가용시간(uptime) - 장애시간(downtime)

### QPS(Query Per Second) 
 
초 당 질의에 걸리는 시간을 추정치

### 데이터 저장소 요구량

트윗 하나당 평균 용량

> 트윗 아이디 : 64byte 
>
> 텍스트 : 140 byte (트위터는 140자 제한이다)
>
> 미디어 : 1 MB

## 가상의 트위터 QPS 와 저장소 요구량 추정 해보기

하루에 애플리케이션을 1.5억명이 이용한다.

50프로의 사용자가 트위터를 매일 사용한다.

평균적으로 각 사용자는 매일 2건의 트윗을 올린다.

미디어를 포함하는 트윗은 10% 정도이다.

데이터는 5년간 보관 된다.

```python

# 입력된 데이터
daily_users = 150000000  # 1.5억 사용자
daily_usage_rate = 0.50  # 50% 사용자가 매일 사용
tweets_per_user_per_day = 2  # 사용자당 하루에 2개의 트윗
media_tweet_percentage = 0.10  # 10%의 트윗이 미디어를 포함
data_storage_duration_years = 5  # 데이터 보관 기간 5년

# 트윗 데이터 크기
tweet_id_size = 64  # 트윗 ID 크기 (bytes)
text_size = 140  # 텍스트 크기 (bytes)
media_size = 1 * 1024 * 1024  # 1MB

# 일일 트윗 수 계산
daily_tweets = daily_users * daily_usage_rate * tweets_per_user_per_day

# QPS 계산
qps = daily_tweets / (24 * 3600)  # 하루는 24시간 * 3600초

# 저장 요구량 계산
daily_text_tweet_size = (tweet_id_size + text_size) * (1 - media_tweet_percentage) * daily_tweets
daily_media_tweet_size = (tweet_id_size + text_size + media_size) * media_tweet_percentage * daily_tweets
total_daily_storage = daily_text_tweet_size + daily_media_tweet_size
total_storage_5_years = total_daily_storage * 365 * data_storage_duration_years

# 총 저장소 요구량을 테라바이트(TB)로 변환하여 출력
total_storage_in_pb = total_storage_5_years / (1024 ** 4)  # TB 단위로 변환
total_storage_in_pb /= 1024  # TB를 PB로 변환

qps, total_storage_in_pb  # QPS와 총 저장소 요구량(PB) 출력
```

QPS (초당 질의 수): 대략 1736.11, 저장소 요구량: 약 25.54 페타바이트 (PB)
