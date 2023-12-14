---
title: 스프링 부트로 배우는 REST API 기초
date: 2023-12-08
categories: [blog, java]
tags: [java, spring_boot, restapi]
---
## Spring Boot Starter Web

Spring Boot Starter Web이 자동 설정(Auto-Configuration)을 처음에 잘 설정 해준다.
    디스패처 서블릿 자동설정, 자바 클래스를 자동으로 JSON으로 바꿔주기, Error 페이지 자동 매핑

### 프론트 컨트롤러 

Spring MVC에서 모든 요청은 디스패처 서블릿이 처리합니다. 이걸 프런트 컨트롤러 패턴이라고 합니다. 
Spring Boot의 자동 설정이(Auto-Configuration) 기본적으로 디스패처 서블릿을 설정해줍니다. 

### 자동으로 JSON Serialize

Spring Boot 는 자바 객체를 기본 설정(Auto-Configuration)을 통해 JSON으로 반환합니다. 

`@ResponseBody + JacksonHttpMessageConverters`

### 예외처리

Spring Boot의 기본 설정(Auto-Configuration)이 ErrorMVCAutoConfiguration 을 기본적으로 설정해줍니다.

## url 파라미터

### 경로 파라미터 (Path Parameter, Path Variable)

`http://localhost:8080/users/{id}`
```
@GetMapping(path = "/users/{id}")
public String getUser(@PathVariable String name){
    // 로직
}
```

### 쿼리 파라미터(Query Parameter)
`http://localhost:8080/users?name={yubin}`
```
@GetMapping(path = "/users")
public String getUser(@RequestParam String name){
    // 로직
}
```

## REST API스럽게 알맞은 응답 반환하기

`ResponseEntity`
org.springframework.http 의 클래스이다.

ResponseEntity 의 메서드를 이용하면 헤더에 RESTAPI 스럽게 사용자에게 필요한 정보를 넣어줄 수 있다.

```
URI location = 응답 헤더에 location 속성 만들어서 줄수 있다.
return ResponseEntity.created(location).build();
```

## 모든 리소스를 대상으로 예외 처리하기

ResponseEntityExceptionHandler 를 확장 & 오버라이드하여 예외핸들러를 생성하면 됩니다.

이 클래스는 Spring MVC가 발생시키는 모든 예외를 처리합니다.

깃허브 코드 : [https://github.com/YubinShin/dispring-be/blob/main/src/main/java/com/yubin/dispring/exception/CustomizedResponseEntityExceptionHandler.java](https://github.com/YubinShin/dispring-be/blob/main/src/main/java/com/yubin/dispring/exception/CustomizedResponseEntityExceptionHandler.java)


### 요청 Validation

spring-boot-starter-validation 패키지로 잘못된 요청에 대한 응답을 쉽게 보낼 수 있다.