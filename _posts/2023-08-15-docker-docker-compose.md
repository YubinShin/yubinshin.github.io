---
title: Docker & Docker-compose
date: 2023-08-17
categories: [troubleshooting]
tags: [CI/CD, docker, docker-compose]
---

# [트러블슈팅] Docker & Docker-compose

## 🤔 Problem

Nest.js 와 PostgreSql 을 함께 사용하면서 병렬적으로 여러 프로그램을 한번에 실행하고 싶다는 생각을 했다.

익숙한 Nginx 사용도 가능하긴 했지만 세팅 과정이 너무 번거로웠다.
좀 더 유지보수에 적합한 방식이 무엇일까 생각해봤고, 지난 프로젝트때 맛만 봤던 도커를 정식으로 공부해보기로 했다.

## 🌱 Solution

### 기본서

![Untitled](https://contents.kyobobook.co.kr/sih/fit-in/458x0/pdt/9791140700943.jpg)

유튜브 강의, 공식문서 등 여러 방법을 시도해보았지만 나는 역시 사전처럼 뚱뚱한 교재 한 권에서 계속 찾아보면서 배우는게 제일 적합했다.
예제가 꼼꼼하게 되어있고, 초반 진입장벽이었던 어려운 개념들을 찾아가며 정독하니 점점 이해가 되었다.
특히 멀티스테이징 빌드, volume 에 관한 설명이 프로젝트 중 문제와 마주쳤을때 큰 도움이 되었다.

### 포트포워딩, Https 인증서 (Traefik)

1. ssl 인증서를 좀 더 편리하게 받고 싶었다.
2. 포트포워딩도 동시에 진행하고 싶었다.

그래서 라즈베리파이에서 실패했던 traefik을 재시도 해서 성공했다. 성공 이유는 좀 더 공부해서 찾아봐야 되겠지만, 일단 될때까지 해서 됐으니 기쁘다!

### 운영체제 오류

내 AWS EC2(ubuntu 20.04) 머신과 아이맥(m1)의 프로세서가 달라서 각자의 운영체제에서 빌드한 이미지가 실행되지 않았다. 기존에 라즈베리파이(arm64) 에서 겪어봤던 문제라 금방 해결할 수 있었다.

```bash
// standard_init_linux.go:228: exec user process caused: **exec format error**

$ docker buildx build --platform linux/amd64 -t shinyubin/fow-be:0.1 . --push
```

### CI&CD 파이프라인

내 m1 아이맥에서 리액트, Nest.js, postgresql 서버, Prisma studio 까지 사용하니, 빌드 시간이 너무 오래 걸리고 중간에 셸 프로세서 연결이 끊겼다는 에러 메시지가 나타나는 경우가 매우 잦아졌다.
아이맥을 너무 굴렸더니 아무리 멀티스테이징 방식으로 도커파일을 리팩토링하고, node-alpine 버전으로 최대한 용량을 낮춰서 만들었는데도 빌드하는데 비용이 너무 많이 들었다.
로컬에서 빌드 1번 하는 데에 최대 200초가 넘어가는데다가, 빌드 후 배포를 수동으로 ec2에 올리면서 SSL이 잘 적용되었는지 왔다 갔다 확인하는 nn회차 시도를 내 컴퓨터에서 하니 API 개발을 진행할 시간이 나지 않았다. 그래서 계속 해보고 싶었던 CI&CD 를 시도했고 성공했다.

자세한 내용은 다음 글로 첨부하겠다.

<aside>
💡 중요한 내용은 이렇게 콜아웃으로 강조하세요

</aside>

## 📎 Related articles

| 이슈명                                          | 링크                                                                |
| ----------------------------------------------- | ------------------------------------------------------------------- |
| 홈에서 Traefik의 장점이 무엇일까요?             | https://svrforum.com/svr/311870                                     |
| Put Wildcard Certificates and SSL on EVERYTHING | https://technotim.live/posts/traefik-portainer-ssl/                 |
| node.js argon2 crash Docker container           | https://techoverflow.net/2023/04/27/how-to-fix-nodejs-argon2-crash/ |
| Prisma Migrate: Deploy Migration with Docker    | https://notiz.dev/blog/prisma-migrate-deploy-with-docker            |
| exec 에러                                       | https://kimjingo.tistory.com/221                                    |
