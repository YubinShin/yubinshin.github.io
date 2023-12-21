---
title: 혼자 공부하는 컴퓨터구조 & 운영체제 - 운영 체제
date: 2023-12-15
categories: [blog, os]
tags: [book]
---

## 운영 체제란? (Operating System)

프로그램 실행에는 CPU, 메모리, 보조기억장치, 입출력장치 등 하드웨어들이 필요합니다.

프로그램 실행에 필요한 모든 요소들을 가르켜 **시스템 자원**(자원)이라고 부릅니다.

이 때, 실행할 프로그램에 필요한 자원을 할당하고, 프로그램이 실행되도록 돕는 프로그램을 **운영 체제**라고 부릅니다.

## 커널 영역

![https://yubinshin.s3.ap-northeast-2.amazonaws.com/2023-12-16-operating-system/ram.png](https://yubinshin.s3.ap-northeast-2.amazonaws.com/2023-12-16-operating-system/ram.png)

RAM 에는 커널 영역(kernal space)과 사용자 영역(user space) 두 공간이 나눠져 있습니다.

운영 체제는 부팅 시 RAM 의 **커널 영역**에 따로 적재됩니다.

나머지 응용 프로그램(Application Software)은 **사용자 영역**에 OS 의 지시에 따라 정리 됩니다.


## OS 의 역할

1. RAM 의 사용자 영역에 응용 프로그램을 주소에 따라 겹치지 않게 적재합니다.
2. 더이상 실행되지 않는 프로그램을 메모리에서 삭제하며 지속적으로 메모리 자원을 관리합니다.
3. CPU 를 사용할 때도 OS가 최대한 스케줄링 방식을 사용해 공정하게 여러 프로그램에 할당합니다.
4. 여러 프로그램이 동시에 자원을 사용하려는 자원 충돌이 발생하지 않게 관리합니다. (워드 vs 메모장이 프린터를 동시에 점유하려는 상황)

## 운영 체제를 알아야 하는 이유

이슈 발생 시 문제 해결의 실마리를 운영체제가 제공한다.

특히 운영체제는 하드웨어가 아닌 Software이기 때문에 사용자와 대화형으로 소통이 가능합니다.

예시 : 메모리 누수 경고 대화상자

![https://yubinshin.s3.ap-northeast-2.amazonaws.com/2023-12-16-operating-system/memoryleak.png](https://yubinshin.s3.ap-northeast-2.amazonaws.com/2023-12-16-operating-system/memoryleak.png)

-----
## 소감

개인적으론 도커와 k8s 를 배우면서 운영 체제를 피부로 느꼈다.

운영체제가 뭔지 알기 전엔 볼륨 마운트나 메트릭 감지, 프로세스 살펴보기 같은 것도 낯설게 느껴졌다. 

하지만 도커 컨테이너도 경량화된 운영체제라는 걸 이해하고 나니 어느 관점에서 지표들을 보면 좋은지 판단 및 해결시간이 빨라졌다 😄 

## 📎 Related articles

| 이슈명                                                                                                                                         | 링크 |
| ---------------------------------------------------------------------------------------------------------------------------------------------- | ---- |
| 도커교과서 도커의 기본적인 사용법                                                                                                              |
| [https://yubinshin.github.io/posts/learn-docker-in-a-month-of-lunches/](https://yubinshin.github.io/posts/learn-docker-in-a-month-of-lunches/) |
| 쿠버네티스 GKE                                                                                                                                 |
| [https://yubinshin.github.io/posts/k8s/](https://yubinshin.github.io/posts/k8s/)                                                               |

<!-- ## 번외

위에 RAM 그림을 그리는데 참고 자료를 보니 램이 4GB 짜리라서 커널과 유저 영역을 1 : 3 으로 나눈 거 같더라. 

내 윈도우 노트북의 메모리 영역이 궁금해서 한번 촬영해보았다.

시작 버튼 옆 검색 창에 `perfmon`, `resmon` 을 검색하면 **리소스 모니터** 가 나온다. 

![https://yubinshin.s3.ap-northeast-2.amazonaws.com/2023-12-16-operating-system/resmon.png](https://yubinshin.s3.ap-northeast-2.amazonaws.com/2023-12-16-operating-system/resmon.png)

> System 이라는 프로세스를 찾았다. 방가방가~

아쉽게도 커널영역과 유저영역을 직접 보려면 따로 고급 도구를 깔아야 한단다.

![https://yubinshin.s3.ap-northeast-2.amazonaws.com/2023-12-16-operating-system/task_manager.png](https://yubinshin.s3.ap-northeast-2.amazonaws.com/2023-12-16-operating-system/task_manager.png)

아쉬운 대로 작업관리자의 성능 탭에서 확인해보니 16GB 램 중 15.7GB 만 사용할 수 있고, 하드웨어 예약 메모리로 302MB를 발견했다.  -->

