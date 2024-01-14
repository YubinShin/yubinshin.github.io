---
title: Network 모델 맛있게 먹기(작성중)
date: 2024-01-04
categories: [blog, network]
tags: []
---

![https://yubinshin.s3.ap-northeast-2.amazonaws.com/2024-01-04-devour-network/models.png](https://yubinshin.s3.ap-northeast-2.amazonaws.com/2024-01-04-devour-network/models.png)


## TCP/IP vs OSI

| 특징 / 모델          | TCP/IP 모델                             | OSI 모델                                                                                                                         |
| -------------------- | --------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- |
| 뜻                   | Transmission Control Protocol           | Open Systems Interconnection                                                                                                     |
| 개발자/기구          | 미국 국방부                             | 국제 표준화 기구(ISO)                                                                                                            |
| 개발 시기            | 1970년대 초 (실제 사용: 1983년)         | 1970년대 말 (공식 발표: 1984년)                                                                                                  |
| 목적                 | 군사적 목적을 위한 통신 표준            | 다양한 시스템 간 원활한 통신을 위한 범용 표준                                                                                    |
| 특징                 | 실용적, 실제 필요와 응용에 중점         | 이론적, 자세한 프레임워크 제공                                                                                                   |
| 현상                 | 인터넷의 기본 프로토콜로 널리 사용      | 주로 교육 및 이론적 참조 모델로 사용                                                                                             |
| 프로토콜의 예        | IP, TCP, UDP                            | 7개 계층 (물리, 데이터 링크, 네트워크 등)                                                                                        |
| 현대적 적용          | 인터넷 및 대부분의 네트워크 통신의 기반 | 네트워크 개념 및 프로토콜 교육에 이용                                                                                            |
| 벤더 중립성          | 초기에는 미국 국방부 중심               | 벤더 중립적, 다양한 시스템을 위한 표준 제시 <br/> IBM 제품과 DEC 제품처럼 다른 제조사에서 만든 컴퓨터도 서로 통신할 수 있게 만듦 |
| 네트워크 모델의 비교 | 실제 인터넷 모델로 발전                 | 이론적 네트워크 모델                                                                                                             |



## ❤️ 물리 계층

이더넷 케이블

![https://yubinshin.s3.ap-northeast-2.amazonaws.com/2024-01-04-devour-network/ethernet_cable_type.png](https://yubinshin.s3.ap-northeast-2.amazonaws.com/2024-01-04-devour-network/ethernet_cable_type.png)

출처 : [https://ivshub.com/network/ethernet-cable-types-utp-stp-and-ftp/](https://ivshub.com/network/ethernet-cable-types-utp-stp-and-ftp/)

## ❤️ 데이터 링크 계층

NIC(네트워크 인터페이스 카드)에 적혀있는 MAC 주소를 보내고 체크함

물리 계층에서 오는 정보의 오류를 감지하고, 필요에 따라 흐름 제어와 오류 수정을 수행합니다.

> MAC 주소란? 
>
> Media Access Control
>
> 랜카드의 고유 주소 / 제조업체의 등록된 식별 번호 
>
> 이더넷과 와이파이를 포함한 대부분의 IEEE 802 네트워크 기술에 네트워크 주소로 사용된다.

![https://commons.wikimedia.org/wiki/File:UMTS_Router_Surf@home_II,_o2-0017.jpg](https://commons.wikimedia.org/wiki/File:UMTS_Router_Surf@home_II,_o2-0017.jpg)

[https://medium.com/@su_bak/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC%EC%97%90%EC%84%9C-mac-%EC%A3%BC%EC%86%8C%EB%9E%80-273dc44587d](https://medium.com/@su_bak/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC%EC%97%90%EC%84%9C-mac-%EC%A3%BC%EC%86%8C%EB%9E%80-273dc44587d)

## 💛 네트워크 계층

IP 주소를 체크함
(Internet Protocol)

ARP 로 IP 주소를 물리적 MAC 주소로 변환함
(Address Resolution Protocol 주소 결정 프로토콜)

ICMP로 네트워크 장치 간의 오류 메시지와 운영 정보를 전송
(Internet Control Message Protocol 인터넷 제어 메시지 프로토콜)


|           | IP                                                         | ARP                                                                   | ICMP                                                                       |
| --------- | ---------------------------------------------------------- | --------------------------------------------------------------------- | -------------------------------------------------------------------------- |
| 영문      | Internet Protocol                                          | Address Resolution Protocol                                           | Internet Control Message Protocol                                          |
| 뜻        | 인터넷 프로토콜                                            | 주소 결정 프로토콜                                                    | 인터넷 제어 메시지 프로토콜                                                |
| 역할      | 데이터를 패킷으로 나누어 인터넷을 통해 전송                | IP 주소를 해당 네트워크 상의 물리적 MAC 주소로 변환                   | 네트워크 장치 간에 오류 메시지와 기타 운영 정보를 전송                     |
| 목적      | 다른 네트워크 장치로 데이터를 안전하게 전송                | 네트워크 내에서 장치의 물리적 주소를 찾기 위함                        | 네트워크 문제를 진단하고 보고                                              |
| 사용 예시 | 웹 브라우징, 이메일 전송 등 인터넷을 통한 모든 데이터 전송 | 네트워크에 ARP 요청을 보내서 특정 IP 주소를 가진 장치의 MAC 를 받아옴 | "핑(ping)" 명령어로 다른 장치에 요청-응답 주고받아 네트워크 연결 상태 확인 |

## 💚 전송 계층

TCP : Transmission Control Protocol

TCP도 주소를 가지고 있으며 이를 포트(port)라고 한다. MAC 주소가 네트워크 카드의 고유 식별자이고 IP가 시스템의 주소라면, 포트는 시스템에 도착한 후 패킷이 찾아갈 응용 프로그램과 통하는 통로 번호라 생각할 수 있다.


UDP : User Datagram Protocol 
전송 제어 프로토콜이라고 부름.
핸드셰이킹 등으로 안정성, 신뢰도 있는 프로토콜 / 
다만 데이터양이 많고 느림.

스트림으로 데이터를 전송하며 데이터가 손상되지 않고 도착했는지 확인하기 위한 체크섬만 있습니다.
UDP는 오류 수정 기능이 거의 없으며 패킷 손실에 대해서도 신경 쓰지 않습니다. 
오류가 발생하기 쉽지만 TCP보다 훨씬 빠르게 데이터를 전송합니다.

https://developer.mozilla.org/ko/docs/Glossary/UDP

사용자 데이터그램 프로토콜이라고 부름.
데이터 무결성 위해 체크섬 및 포트번호 제공공속도가 빠른 프로토콜(게임, 영상에 많이 사용) /
다만 보안과 신뢰성 낮음.무연결 통신, 핸드셰이킹 대화 수단 X전달, 순서 또는 중복에 관한 보호가 보장되지 않습니다.

데이터 단위가 에러 없이 전송되도록 관리하고 메시지 분할과 재조립, 프로세스 간 정보 흐름 조정 등을 수행하는 것의 총칭. 통신을 하는 두 사용자의 종단(endtoend) 간에 신뢰성 있는 데이터 전송을 보장해 준다.

상위 계층들이 데이터 전달의 유효성이나 효율성을 신경 쓰지 않도록 해준다.
전송 계층은 시퀀스 넘버 기반의 오류 제어 방식을 사용하여 특정 연결의 유효성을 제어하며, 일부 프로토콜은 상태 개념이 있고(stateful) 연결 기반(connection oriented)이다. 이는 전송 계층이 패킷들의 전송이 유효한지 확인하고 전송 실패한 패킷들을 다시 전송한다는 것을 뜻한다.

[네이버 지식백과] 전송 계층(4계층) (정보 보안 개론, 2013. 6. 28., 양대일)

## 💙 세션 계층


## 💙 표현 계층

코드 간의 번역을 담당함. 
헤더 정보에 데이터 암호화 방식과 압축 방식에 대한 설명을 붙인다.
보낼 때는 OSI 표준 표현 방식으로 변경하고 받을 땐 자기 시스템에 맞게 번역함

## 💙 응용 계층

FTP : File Transfer Protocol

DHCP : 동적 호스트 설정 프로토콜 
https://opentutorials.org/course/3265/20039 

[네이버 지식백과] TCP [Transmission Control Protocol] - 전송 제어 규약 (지형 공간정보체계 용어사전, 2016. 1. 3., 이강원, 손호웅)

[네이버 지식백과] 개방형 시스템 간 상호 접속 [Open Systems Interconnection, 開放型system間相互接續] (국방과학기술용어사전, 2021. 05. 31.)