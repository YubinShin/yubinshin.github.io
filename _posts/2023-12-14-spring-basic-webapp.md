---
title: 스프링 부트로 배우는 기본적인 웹앱 만들기 
date: 2023-12-12
categories: [blog, java]
tags: [java, spring_boot, restapi, mvc]
---


## 준비

Spring Initializr

| Maven | 빌드 툴 |
|  자바 버전 | 자바 17 이상|
| Spring Web REST API  | 어노테이션 제공 |
| Apache Tomcat  | 웹 앱 컨테이너를 제공한다. (서블릿, http 처리기, SSL/TLS) |
| Spring Data JPA | DB 접근 |
| Spring Boot DevTools  | 핫 리로딩 |

<!-- > Apache Tomcat
> Apache Tomcat은 Java 언어로 작성된 오픈 소스 구현체이며, 주로 Java Servlet, JavaServer Pages (JSP), Java Expression Language를 실행하는 웹 컨테이너(Web Container) 또는 서블릿 컨테이너(Servlet Container)로 사용됩니다. 주요 기능과 역할은 다음과 같습니다:
> 웹 서버와 통신: Tomcat은 HTTP 요청을 받고, 웹 서버처럼 정적 자원을 제공할 수 있습니다. 또한, Java EE 기반의 웹 애플리케이션을 지원합니다.
> 서블릿과 JSP 실행: 가장 중요한 기능 중 하나로, Tomcat은 클라이언트의 요청에 대해 서블릿이나 JSP를 실행하여 동적인 컨텐츠를 생성하고 응답합니다.
> 컨텍스트와 워크플로우 관리: 애플리케이션의 생명주기를 관리하고, 다양한 애플리케이션 및 서버 관련 설정을 핸들링합니다.
> 보안: SSL/TLS 지원을 통해 보안 통신을 제공하며, 사용자 인증 및 권한 부여를 관리합니다.
> 통합 및 확장성: 다양한 백엔드 시스템과의 통합이 용이하며, 플러그인 및 추가 컴포넌트를 통해 기능을 확장할 수 있습니다.
> 이러한 기능들 덕분에 Tomcat은 Java 기반의 웹 애플리케이션 개발과 배포에 있어 널리 사용되는 중요한 컴포넌트입니다. Spring과 같은 프레임워크와 함께 사용될 때, 개발자는 웹 애플리케이션을 보다 효율적으로 구축, 테스트, 배포할 수 있습니다. -->

## Spring Web REST API 어노테이션

```
@RestController

@RequestMapping(method=RequestMethod.GET)
@GetMapping("/jpa/users")
@PostMapping
@PutMapping
@PatchMapping
@DeleteMapping

@ResponseBody 

@RequestParam String name
```

<!-- ## Apache Tomcat
jsp 쓸때 사용
MVC 에서 V는 jsp view 였다!
```

``` -->

## Logging > slf4j

우리가 사용할 Logger는 slf4j입니다

```
private Logger logger = LoggerFactory.getLogger(getClass());

logger.log();
logger.debug();
logger.warn();
logger.info();
logger.error();
```

## 자바 웹 앱의 역사

> MVC 에서 V는 jsp view 였다!

1. 모델 1 아키텍처
   
    모든 코드를 view 계층에 작성

    모든 로직이 jsp에 있습니다. (당시엔 컨트롤러가 없었음)

    유지보수 어려웠음
2. 모델 2 아키덱처 (MVC)
    - 모델 : 뷰를 생성하는 데이터를 받아옴.
    - 뷰 : 정보를 사용자에게 보여줌
    - 컨트롤러 | 서블릿 : 전체 흐름을 제어함
    유지보수가 더 쉬웠다.
    하지만 모든 컨트롤러에서 사용하는 공용 로직을 분리하고 싶었음.
3. 모델 2 아키덱처 (프론트 컨트롤러가 있는)
    - 중앙 컨트롤러(Front Controller): 이 패턴에서 모든 클라이언트 요청은 중앙의 프론트 컨트롤러(예: Spring의 DispatcherServlet)를 통과합니다. 이것은 웹 애플리케이션의 흐름을 한 곳에서 관리하여 일관성과 관리 용이성을 향상시킵니다.
    - 요청의 분배: 프론트 컨트롤러는 들어오는 요청을 분석하고, 해당 요청을 처리할 적절한 컨트롤러로 전달합니다. 이는 MVC 패턴의 컨트롤러에 해당합니다.
    - 컨트롤러의 처리: 각 컨트롤러는 비즈니스 로직을 수행하고, 처리 결과와 함께 뷰 이름을 프론트 컨트롤러에 반환합니다.
    - 뷰 리졸버(View Resolver): 프론트 컨트롤러는 반환된 뷰 이름을 뷰 리졸버에 전달합니다. 뷰 리졸버는 이 이름을 실제 뷰 경로(예: JSP 파일)로 변환합니다. 여기서 prefix와 suffix는 뷰 이름을 완전한 경로로 변환하는 데 사용됩니다.
    - 최종 응답: 프론트 컨트롤러는 매핑된 뷰(예: 알맞은페이지.jsp)를 사용하여 클라이언트에게 최종 응답을 생성하고 전송합니다. 이 응답은 HTML, JSON, XML 등 다양한 형식으로 될 수 있으며, 주로 JSP 파일을 통해 생성된 HTML이 사용됩니다.


## 세션

원래 HTTP 요청(Request Scope)은 한번 요청, 응답이 끝나면 저장되지 않고 메모리에서 바로 사라진다.(무상태)

하지만 로그인했는지 여부를 체크하고 싶었다. 그래서 세션(Session Scope)가 등장했다.

`@SessionAttributes("모델속성명")` 를 사용하면 그 요청을 서버 메모리에 저장해서 여러 요청에 걸쳐 값을 유지할 수 있다. (그 값을 사용하려는 모든 컨트롤러에 적어줘야 한다.)

> 주의할 점은 @SessionAttributes는 세션을 전반적으로 관리하는 것이 아니라, 오직 특정 모델 속성에 한정하여 사용됩니다.
> Spring Session, Apache Shiro, Java EE (Jakarta EE), Redis, Hazelcast 등의  프레임워크와 라이브러리들은 개발자가 세션 관리에 대한 세부적인 구현을 신경 쓰지 않고도, 안전하고 효율적인 세션 관리를 할 수 있게 도와줍니다.