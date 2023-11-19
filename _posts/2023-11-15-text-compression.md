---
title: gzip 을 사용한 텍스트 압축
date: 2023-11-15
categories: [troubleshooting, performance_improvement]
tags: [gzip, fastapi, nginx]
---

## 🤔 Problem

라이트하우스를 사용해 맛이슈의 성능을 평가해보았을때 텍스트 압축이 필요하다는 메시지가 나타났다.

그래서 가장 많이 쓰인다는 [gzip](https://www.gnu.org/software/gzip/) 이라는 방식을 이용하여 압축을 시도해보았다.

<details markdown="block"><summary>gzip (GNU zip) 이 가장 많이 쓰이는 이유는? </summary>


[GNU 웹 페이지에 GIF 파일을 사용하지 않는 이유](https://www.gnu.org/philosophy/gif.ko.html)

기존에 표준이었던 Compress 압축도구는 LZW 압축알고리즘을 사용했다고합니다.

Unisys라는 기업이 LZW 압축 알고리즘에 대한 특허를 가지고 있었는데, 

로열티가 부담되던 개발자들과 기업은 해당 특허에 구속되지 않는 압축방식이 필요했다고 합니다.

gzip은 LZW 특허와 관련이 없는 DEFLATE 압축 알고리즘을 사용했습니다. 

Unisys의 LZW 특허 문제를 회피하면서 더 나은 압축 효율과 속도를 제공하는 새로운 도구로 gzip이 등장하였고,

이로 인해 gzip은 특히 UNIX 및 리눅스 기반 시스템에서 빠르게 표준 압축 도구로 자리잡게 되었습니다.

</details>

## 🌱 Solution

### 첫번째 시도, Nginx (실패)

현재 리버스프록싱, 로드밸런싱용으로 사용하고 있는 Traefik 에는 기본적으로 텍스트 압축 기능이 포함되어있지 않다고 한다.

그래서 해당 기능이 있다는 Nginx 를 컨테이너로 띄워 gzip을 설정해보았다.

(Nginx 사용시 systemctl 설정때문에 로컬 os 에 설치하는 걸 선호하긴 하나, 테스트 단계라서 가볍게 사용하기 위해 컨테이너로 설정했다.)

#### 1. Nginx 설정파일 추출

  - 임시 Nginx 컨테이너 실행
  
    내가 원하는 설정을 활성화 하기 위해서 임시로 아무런 설정도 하지 않은 nginx 컨테이너를 실행한다.

    ```sh
    $ docker run -d --name temporary-nginx nginx
    ```

  - Nginx 설정 복사
  
    방금 실행한 `temporary-nginx` 컨테이너에서 설정 관련 부분들만 쏙 빼서 로컬 머신에 복사한다.
  
    📄 파일 `./nginx/nginx.conf`  
    📁 폴더 `nginx/conf.d` 

    ```sh
    $ docker cp temporary-nginx:/etc/nginx/nginx.conf nginx/nginx.conf

    $ docker cp temporary-nginx:/etc/nginx/conf.d ./nginx
    ```

#### 2. nginx.conf 에 gzip 설정

  - 복사본 확인
  
    이제 우분투에서 ls 를 해보면 nginx 폴더에 해당 파일들이 복사된 것을 확인할 수 있다.

    ```sh
    ubuntu:~$ ls
    docker-compose.yml  letsencrypt  nginx
    ```
    
  - 복사본에 설정파일에 gzip 설정
  
    nginx.conf 파일을 쭉 내리다보면 하단에 `#gzip  on;`이라는 구절이 보인다.

    해당 부분에 gzip 설정을 해주면 된다.

    ```py
        gzip on;
        gzip_min_length  10240;
        gzip_buffers  32 32k;
        gzip_comp_level 9;
        gzip_types    text/plain application/x-javascript text/xml text/css                   
        application/json;
        gzip_vary on;    
        include /etc/nginx/conf.d/*.conf;
    ```

    <details markdown="block"><summary>상세 설명 </summary>
    ```js
    /** gzip 압축 기능 활성화*/
    gzip on; 

    /** 최소 10,240바이트(약 10KB) 크기의 콘텐츠에만 압축이 적용됨*/
    gzip_min_length  10240; 

    /** 압축 처리에 사용될 메모리 버퍼의 수와 크기를 정의*/
    gzip_buffers  32 32k; 

    /** 압축 수준을 설정. 압축 수준이 높을시 자원을 많이 사용하지만 요즘은 하드웨어 좋아져서 가장 높은 수준인 9로 설정했다. */
    gzip_comp_level 9; 

    /** 일반 텍스트, JavaScript, XML, CSS, JSON 파일에 압축이 적용 */
    gzip_types text/plain application/x-javascript text/xml text/css application/json; 

    /** Vary: Accept-Encoding 헤더를 응답에 추가하여,압축된 버전과 압축되지 않은 버전의 콘텐츠가 클라이언트나 중간 프록시에 의해 별도로 캐시될 수 있도록 한다. */
    gzip_vary on;    

    /** `/etc/nginx/conf.d/` 디렉토리 내의 모든 `.conf`확장자를 가진 파일을 현재 설정 파일에 포함시킵니다. */ 
    include /etc/nginx/conf.d/*.conf; 

    ```
    </details>   

#### 3. 볼륨 마운트 & Nginx 컨테이너 실행

수정한 설정파일과 폴더를 nginx 컨테이너의 볼륨으로 만들어 마운트한다.

  ```yml
      version: "3.8"

      services:
        matissue-be:
          image: shinyubin/matissue-be:latest
        <중략>
        nginx:
          image: nginx:latest
          ports:
            - "8080:80"
          volumes:
            - ./nginx/nginx.conf:/etc/nginx/nginx.conf
            - ./nginx/conf.d:/etc/nginx/conf.d
          depends_on:
            - matissue-be # 메인 백엔드가 켜져야만 nginx 가 켜질수 있게 보장한다.
  ```

#### 4. 하지만 아쉽게도 실패했다..
   
  아무리 API 요청을 해도 gzip 으로 압축했다는 헤더가 보이지 않는 것이다.

  `content-encoding: gzip, vary: Accept-Encoding` 가 보여야 하는데 보이지 않고 있다. 🤦‍♀️

  그런데 응답을 보니 server가 **uvicorn**으로 되어있었다.

> uvicorn 은 fastapi 서버를 실행해주는 ASGI(Asynchronous Server Gateway Interface) 서버 구현이다.


### 두번째 시도, FastAPI

  Server 가 uvicorn 이라는 데서 착안해 fastAPI 쪽 공식문서에 GZip 을 검색해보니, 미들웨어가 있었다.  대박사건~
  
  [GZipMiddleware](https://fastapi.tiangolo.com/advanced/middleware/#gzipmiddleware)

  생각보다 굉장히 간단했는데, `main.py` 에 fastapi 기본 제공인 `GZipMiddleware`를 import 하고, `app.add_middleware` 에 GZip 설정을 추가해주면 됐다.

  ```py
  from fastapi import FastAPI
  from fastapi.middleware.cors import CORSMiddleware
  from fastapi.middleware.gzip import GZipMiddleware
  from API.routes.api_routes import api_router

  app = FastAPI()

  origins = [
      "https://www.matissue.com/",
      "https://www.matissue.com",
      "https://matissue.com/",
      "https://api.matissue.com",
      "https://api.matissue.com/",
  ]

  app.add_middleware(
      CORSMiddleware,
      allow_origins=origins,
      allow_credentials=True,
      allow_methods=["*"],
      allow_headers=["*"],
  )

  app.add_middleware(
      GZipMiddleware, minimum_size=1000
  )

  app.include_router(api_router, prefix="/api")
  ```

  > 💡주의
  >
  >  이렇게 하나의 add_middleware 메서드엔 하나의 미들웨어 인스턴스만 들어가더라. 
  > 아래와 같은 코드는 에러가 난다.
  >
  >  app.add_middleware(
  >    CORSMiddleware,
  >    GZipMiddleware, minimum_size=1000
  >   )

### 성공

Fastapi Gzip 미들웨어 설정 후 서버를 재시작하니 아래와 같이 정상적으로 `content-encoding: gzip, vary:Accept-Encoding`를 확인할 수 있었다.

```sh
  root@yubin:~#
    curl -X GET \
      -H 'Accept-Encoding: gzip' \
      -D - -o /dev/null -s \
      'https://api.matissue.com/api/recipes/?page=1&limit=160'
    HTTP/2 200 
    content-encoding: gzip
    content-type: application/json
    date: Wed, 15 Nov 2023 04:55:27 GMT
    server: uvicorn
    vary: Accept-Encoding
    content-length: 115854
```

라이트 하우스 점수도 92점으로 올랐다.

  ![image](https://github.com/YubinShin/YubinShin.github.io/assets/68121478/c4a8f690-f0a7-4d7d-a128-4446162e321b)

## 📎 Related articles

| 이슈명                        | 링크                                                                                                                                                     |
| ----------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| NGINX Docker container 만들기 | [https://velog.io/@woo94/NGINX-Docker-container-%EB%A7%8C%EB%93%A4%EA%B8%B0](https://velog.io/@woo94/NGINX-Docker-container-%EB%A7%8C%EB%93%A4%EA%B8%B0) |



