---
title: 스프링 부트로 배우는 REST API 고급
date: 2023-12-12
categories: [blog, java]
tags: [java, spring_boot, restapi]
---
## Documentation

Open API 와 Swagger

springdoc-openapi-starter-webmvc-ui 패키지를 사용한다.

## Content Negotiation(콘텐츠 협상)

Jacksons를 사용한다. 헤더로 요청시 원하는 방식으로 응답해 줄 수 있다.

1. XML 과 JSON 중 뭘 받을지 고를 수 있다.
Accept-header : xml 

2. Language 를 고를 수 있다.
Accept-Language : en 등 

## 국제화 i18n

Locale, messageSource 등으로 지역마다 다른 응답을 줄 수 있다.

깃허브 소스 : [https://github.com/YubinShin/dispring-be/blob/0fb8c39d74c17ce5717528a304e4a5d0630043b9/src/main/java/com/yubin/dispring/ui/UIController.java](https://github.com/YubinShin/dispring-be/blob/0fb8c39d74c17ce5717528a304e4a5d0630043b9/src/main/java/com/yubin/dispring/ui/UIController.java)

## 버저닝

트위터, 아마존, 마이크로소프트, Github 등 다양한 기업에서 사용하는 여러 방식이 있다.

```
 /**URI 방식 버전 관리
     * Twitter Style
     * */
    @GetMapping("/v1/person")
    public PersonV1 getFirstVersionOfPerson() {
        return new PersonV1("Bob Charlie");
    }

    @GetMapping("/v2/person")
    public PersonV2 getSecondVersionOfPerson() {
        return new PersonV2(new Name("Bob", "Charlie"));
    }

    /**Request Parameter 방식 버전 관리
     * Amazon Style
     * */
    @GetMapping(path = "/person", params = "version=1")
    public PersonV1 getFirstVersionOfPersonRequestParameter() {
        return new PersonV1("Bob Charlie");
    }

    @GetMapping(path = "/person", params = "version=2")
    public PersonV2 getSecondVersionOfPersonRequestParameter() {
        return new PersonV2(new Name("Bob", "Charlie"));
    }

    /**(Custom)Header 방식 버전 관리
     * Microsoft Style
     * */
    @GetMapping(path = "/person/header", headers = "X-API-VERSION=1")
    public PersonV1 getFirstVersionOfPersonRequestHeader() {
        return new PersonV1("Bob Charlie");
    }

    @GetMapping(path = "/person/header", headers = "X-API-VERSION=2")
    public PersonV2 getSecondVersionOfPersonRequestHeader() {
        return new PersonV2(new Name("Bob", "Charlie"));
    }

    /**
     * Media Type 방식 버전 관리
     * Github Style
     * */
    @GetMapping(path = "/person/accept", produces = "application/vnd.company.app-v1+json")
    public PersonV1 getFirstVersionOfPersonAcceptHeader() {
        return new PersonV1("Bob Charlie");
    }

    @GetMapping(path = "/person/accept", produces = "application/vnd.company.app-v2+json")
    public PersonV2 getSecondVersionOfPersonAcceptHeader() {
        return new PersonV2(new Name("Bob", "Charlie"));
    }
```

## HATEOAS

HATEOAS란, REST API의 구현을 더욱 효율적이고 유연하게 만들어주는 설계 방식이다.
클라이언트가 API 응답에 다음에 할 수 있는 행동에 대한 정보(링크 등)를 포함시킨다.


## 응답 커스터마이징 
Jacksons 라는 모듈로 REST API 응답을 커스터마이징 할 수 있습니다.

**자바 객체**를 **스트림(JSON, XML)**로 변환하는 걸 **직렬화**라고 합니다.

### 속성명 커스터마이징
    @JsonProperty : 응답 필드의 이름을 커스터마이징 할 수 있습니다.(db에 있는 필드명은 name 이지만 응답은 user_name 으로 하고 싶은 경우)

### 반환할 응답 내용 필터링 하기
    원한다면 선택한 필드만 응답으로 반환할 수 있습니다. (비밀번호를 반환하고 싶지 않은 경우)

    1. 정적필터링
    - @JsonIgnore : 클래스 내부 속성에 붙여줍니다.
    - @JsonIgnoreProperties : 클래스 자체에 붙이고 매개변수에 필터링하고 싶은 속성명을 적어줍니다.

    2. 동적필터링
    - REST API 엔드포인트마다 필터링을 다르게 하고 싶을때 사용합니다.
    - REST API **메서드마다** 정의해줘야하기때문에 데이터뿐 아니라 필터링방식도 정의해줘야 합니다.

    `MappingJacksonValue` : 이 클래스로 반환하고자 하는 자바 객체를 감싸주면 직렬화하면서 필터링 로직을 추가할 수 있어집니다.
    `SimpleBeanPropertyFilter` : 필터를 만드는 클래스입니다. 인스턴스를 이용해 필터링할 속성을 지정합니다.
    `FilterProvider` : "필터이름" 과 SimpleBeanPropertyFilter로 만든 필터를 적어줍니다.
    예 ) new SimpleBeanPropertyFilter().addFilter("SomeBeanFilter", filter);

    `@jsonfilter("필터이름")` 를 응답 클래스에 정의해주면 필터가 연결되어 잘 작동합니다.

## 모니터링

spring boot actuator를 사용한다.

스프링 부트 프로젝트를 운영환경에서 모니터링하고 관리해줌.
    
많은 엔드포인트를 제공함.
`/actuator` : 전체 hateoas 를 제공함. src\main\resources\application.properties 에서 진행가능 > `management.endpoints.web.exposure.include=*`

`/beans` 라는 엔드포인트는 애플리케이션에 spring bean 전체 목록을 제공함
        
`/health` 라는 엔드포인트는 애플리케이션 상태정보를 제공함

`/metric` 이라는 엔드포인트는 애플리케이션의 메트릭을 제공함

`/mappings` 이라는 엔드포인트는 요청 매핑에 관한 세부정보를 볼 수 있음

> 메트릭 : 시간이 지남에 따라 변화하는 데이터를 의미한다. 메모리 사용률, CPU 사용률, 스레드 사용률 등등.. 시간에 따른 추이를 추적할 가치가 있는 데이터를 메트릭(Metric)이라 부른다.

## API 테스트

HAL Explorer 를 사용한다.

HAL을 사용하는 RESTful API 라면 HAL Explorer 를 통하여 API 를 테스트하거나 실행해볼 수 있다.
기술자가 아닌 사람도 Hal explorer 를 통하여 API 를 사용할 수 있다 (PostMan 과 같은 툴을 쓰지 않더라도)

pom.xml에 Spring Boot HAL Explorer 패키지 설치 시 `/` 페이지에 HAL explorer 가 자동으로 생깁니다.

> HAL (JSON Hypertext Application Language)
>
> HAL은 JSON 하이퍼텍스트 애플리케이션 언어의 약자로 일관되고 쉽게 **API 리소스 간 하이퍼링크를 제공**하는 간단한 포맷입니다.