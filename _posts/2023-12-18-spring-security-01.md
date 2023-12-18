---
title: 스프링 부트로 배우는 Spring Security-1
date: 2023-12-18
categories: [blog, java]
tags: [java, spring_boot, security]
---

## 인증과 인가

### 인증 

사용자와 ID 정보가 일치하는지 확인하고 적절한 사용자인지 판단하는 것.

#### 인증방법

 1. 사용자가 기억할 수 있는 정보 활용 (id, pw 등)
 2. 사용자의 소유물을 기반으로 활용(지문, 홍채, 모바일 기기, 여권 등)

### 인가

사용자를 인증하고 나서 적절한 **엑세스 권한을 가지고 있는지 판단**한다.

비행기에 탑승하려면 여권, 탑승권, 사원증등이 필요하다. 

- 인증 :일단 여권이 있어야만 비행기에 탑승할 수 있다.- 인가 : 승객의 탑승권은 객실에만 출입할 수 있다. 
- 인가 : 승무원의 사원증은 보안실이나 기계실, 조종실에 출입할 수 있다.

## 보안의 6 원칙 

1. 아무것도 신뢰하지 않기(Trust Nothing): **시스템에 들어오는 모든 요청과 데이터를 검증**해야 합니다. 신뢰를 기본 전제로 하지 않습니다.
   
2. 최소 권한 할당(Assign Least Privileges): 사용자나 시스템 요소에 필요한 **최소한의 권한만을 부여**합니다. 이는 프로젝트 시작부터 고려해야 하는 보안 측면입니다. 각 사용자에게 필요한 사용자 역할과 액세스 권한을 명확히 정해야 합니다. 모든 레벨에서 가능한 한 최소한의 권한을 할당하세요. 애플리케이션 레벨과 인프라 레벨 모두 마찬가지입니다. 데이터베이스, 서버 등, 시스템을 구성하는 모든 요소에 대한 액세스를 최소화하세요.
   
3. 완벽한 조율 구축(Have Complete Mediation): 모든 요청을 일관된 보안 절차를 통해 검증하며, 요새의 정문처럼 **유일한 입구(보안 필터)**로 관리하면서 요청이 들어올 때마다 액세스 권한을 확인해야 합니다.
   
4. 심층적인 방어 구축(Have Defense In Depth): 보안은 여러 계층에서 구현되어야 하며, **각 레이어는 서로 다른 보안 메커니즘을 제공해야합니다.** 전송 레이어와 네트워크 레이어, 인프라 레이어마다 보안을 구현해야 합니다. 운영 체제 레벨과 애플리케이션 레벨마다 보안을 구현해야 합니다.
   
5. 메커니즘의 효율성(Have Economy Of Mechanism): 보안 아키텍처는 **가능한 한 간단하게** 유지해야합니다. 간단한 시스템을 보호하는 것이 더 쉽습니다.

6. 설계의 개방성 보장(Ensure Openness Of Design): 개방형 보안 설계와 표준을 사용합니다. JWT 등 **잘 알려진 표준을 사용하세요.** 이러한 표준 원칙을 따른다면 보안 결함을 식별하고 해결하기가 더 수월해집니다.

## Spring Security

spring security 는 스프링 생태계에서 가장 인기있는 보안 프로그램이다.

웹 앱, REST API, 마이크로서비스 등을 보호한다.

알아야하는 내용이 많아 학습 곡선이 가파르다.
- 필터체인
- 인증 관리(Authentication Managers)
- 권한 부여(Authentication Providers)

하지만 매우 유연하고 customizable 하다.
- 기본적으로 모든 걸 보호한다.
- 필터 체인을 통해 적절히 커스텀 가능하다.


### Spring Security 의 작동 원리

요청 -> 스프링 시큐리티 -> 디스패처 서블릿 -> 컨트롤러

1. 디스패처 서블릿으로 들어가는 모든 요청을 인터셉트한다.
2. 필터 체인으로 execute한다.
   - **인증(Authentication)**: 적절하고 유효한 사용자인가?
   - **권한 부여(인가) (Authorization)** : 유저가 적절한 접근 권한을 가지고 있는가?
   - **크로스 오리진 리소스 쉐어링(CORS)**: 다른 오리진 간 리소스 공유 여부 설정
   - **사이트 간 요청 위조 감지 (CSRF)** : 스프링 시큐리티를 쓰면 CSRF 보호가 모든 라우터에 작동한다. 사용자가 웹사이트에 로그인하면 사용자 세션이 생성되는데요, 이 사용자 세션은 브라우저의 쿠키를 사용해 식별됩니다. 사용자가 이 웹사이트에서 로그아웃하지 않은 채 악성 웹사이트로 이동하면, 악성 웹사이트에서는 사용자의 이전 인증 정보 브라우저에 있는 쿠키를 이용할 수 있어요.
   - 기본 로그인, 로그아웃 필터 제공
   - **ExceptionTranslationFilter** : 인증, 인가 관련 예외를 적절하게 HTTP 리소스로 번역해서 보내줌.
  
3. 필터를 특정 순서에 따라 통과 시킨다.
    (1) 기본 체크 필터 : CORS, CSRF
    (2) 인증 필터 : Authentication
    (3) 인가 필터 : Authorization

## Spring Security의 기본 설정

Spring Security 는 자바 17 이상 부터 호환된다.

### Spring Security 프로젝트 의존성 설치

Spring Initializr 
[x] gradle
[x] spring-boot-starter-web 
[x] spring-boot-starter-security
[x] spring-boot-devtools

## 완벽한 조율 구축(Have Complete Mediation)

   모든 요청을 검사하고 보안 필터를 구현하는 것을 의미합니다.
   기본적으로 Spring Security는 모든 리소스를 보호합니다. 사용자가 존재하지 않는 URL에 접근하려고 하면, Spring Security는 먼저 로그인 페이지로 리다이렉트합니다. 사용자가 로그인하면, 그 후에 해당 리소스가 실제로 존재하는지 여부를 확인합니다. 이는 외부 사용자가 **적절한 자격증명 없이는 특정 리소스의 존재 여부조차 알 수 없게 만들어**, 보안을 강화합니다.

## 인증 방법

### Form Based Authentication(폼 기반 인증)

- default login, logout 페이지와 폼을 제공하고, /logout api 를 제공합니다.
- 로그인 폼을 submit 하면 Spring Security 는 기본적으로 **세션 쿠키**를 사용합니다.
- Cookie 에 세션 아이디를 담는 방식입니다. (JSESSIONID:1234567)
- 사용자가 로그인하면 세션 ID가 생성되고 이 세션 ID는 쿠키로 **이후의 모든 요청과 함께 전송됩니다.**

### Basic Authentication (기본 인증)

#### 설정 방법

application.properties 에 지정

```
  spring.security.user.name=in28minutes 
  spring.security.user.password=dummy
```

#### 상세
  - username 과 password 를 Base64 로 인코딩한다.
  - RESTAPI 요청 헤더에 `Authorization : Basic + 인코딩값` 을 담아 보낸다.
  - 가장 간단하나 단점(flaws)이 많아 프로덕션형으로 사용되지 않음.

#### 단점 

  - Base64 는 디코딩하기가 매우 쉽기 때문에 Authorization 헤더만 훔치면 누가 username 과 password 를 훔칠 수 있음.
  - 사용자의 접근 권한, 역할 등 사용자의 권한 부여 정보가 없다.
  - 만료일이 없어 영구적으로 사용 가능하다.

### CSRF (**C**ross-**S**ite **R**equest **F**orgery)

  1. 은행 웹사이트에 접속
  2. 사용자 로그인 시 웹브라우저에 쿠키 생성 및 저장
  3. 사용자가 로그아웃하지 않고 악성 웹사이트(Malicious website)로 이동
  4. 브라우저에 남아있던 쿠키를 가지고 은행 웹사이트에 요청

#### CSRF 방지 방법 

1. 동기화 토큰 패턴

   모든 요청마다 토큰을 새롭게 생성하는 방식.
   기존에는 사용자가 세션을 생성할때마다 세션 id를 새롭게 만들었지만, 보안을 위해 사용자가 수행하는 작업(request) 마다 토큰을 생성한다.
   Spring Security 기본 설정에서는 **모든 POST와 PUT 요청에 CSRF 토큰**이 필요합니다.

   POST, PUT 요청 시 요청헤더에 X-CSRF-TOKEN 을 담아 보내야 합니다.

   다만 상태(세션)를 저장하지 않는 REST API 의 경우 CSRF 가 필요하지 않습니다. 
   상태가 없는 REST API 의 경우 CSRF를 사용 해제하는 게 좋습니다.

2. SameSite 쿠키 사용하기
   
   `Set-Cookie : SameSite=Strict` 를 사용하는 겁니다. 해당 설정 시 같은 url 을 가진 사이트로만 쿠키가 전송 됩니다.

   이 프로퍼티는 Spring Boot 2.6부터 지원됩니다
  
   application.properties

   ```
    spring.main.banner-mode=off
    logging.pattern.console= %d{MM-dd HH:mm:ss} - %logger{36} - %msg%n

    spring.security.user.name=in28minutes
    spring.security.user.password=dummy
    server.servlet.session.cookie.same-site=strict
   ```

#### CSRF 를 해제하려면?

1. Session Creation Policy 를 `STATELESS` 로 설정한다.
2. csrf 를 disable 로 설정한다. `http.csrf(csrf -> csrf.disable());` 


```java

// @Order 삭제
// 람다 방식으로 작성

@Configuration
public class BasicAuthSecurityConfiguration {
	
	@Bean
	SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
		
		http.authorizeHttpRequests(
						auth -> {
							auth.anyRequest().authenticated();
						});
		
		http.sessionManagement(
						session -> 
							session.sessionCreationPolicy(
									SessionCreationPolicy.STATELESS)
						);
		
		//http.formLogin();
		http.httpBasic(withDefaults());
		
		http.csrf(csrf -> csrf.disable());
		
		return http.build();
	}
}

```

### CORS (Cross Origin Resource Sharing)

브라우저는 원칙적으로 다른 url 간 AJAX 요청을 금지합니다.

전통적인 MVC 방식(Spring 안에서 JPA 사용)일땐 FE, BE 가 다른 오리진에 있는게 이상한 일이었거든요.

그런데 요즘은 FE, BE 를 다른 오리진에서 켜는 경우가 많기 때문에 리소스 공유가 필요해졌습니다.

Spring MVC 사용시 글로벌 설정, 로컬 설정(컨트롤러 별) 2가지 옵션이 있습니다.

1. 글로벌 설정

   `WebMvcConfigurer` 타입의 클래스에 `@Bean` 을 붙이고 `addCorsMappings` 라는 메서드를 추가합니다.

   ```java
    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/**")
                .allowedOrigins("*")
                .allowedMethods("*")
                .allowedHeaders("*")
                .allowCredentials(true);
    }
   ```

2. 로컬 설정
   특정 요청 메서드나 특정 컨트롤러 클래스에 `@CrossOrigin` 을 추가합니다.

   `@CrossOrigin(origins="https://google.com")` 이런 식으로 특정 오리진만 허용하게 지정할 수도 있습니다.

## 자격증명 저장하기

### 인메모리에 저장하기

- 프로덕션으로는 추천되지 않습니다. (규모확장성 ㅜㅜ)
- 인메모리 설정은 application.properties 에 설정한 값을 기준으로 합니다.

  ```
    spring.security.user.name=in28minutes
    spring.security.user.password=dummy
  ```

#### 메모리에 여러 자격증명을 저장하려면? 

여러 자격증명을 저장하는 방법도 알아보겠습니다.

1. application.properties 에 있는 user.name, user.password 를 **주석처리** 합니다.
   
2. 여러 사용자를 생성하겠습니다.

   프로젝트에서 사용하는 Spring Security Configuration 클래스에 UserDetailsService 라는 메서드를 Bean으로 선언합니다. 그리고 내부에 return new InMemoryUserDetailsManager 를 적어줍니다.

   .roles() 사용의 best practice 는 Enum 을 사용하는 것입니다. (Enum.user, Enum.admin 등)

   ```java
    import org.springframework.security.core.userdetails.User;
    import org.springframework.security.core.userdetails.UserDetails;
    import org.springframework.security.core.userdetails.UserDetailsService;
    import org.springframework.security.provisioning.InMemoryUserDetailsManager;

    @Configuration
    public class BasicAuthSecurityConfiguration {

        // 중략

        @Bean
        public UserDetailsService userDetailService() {
            
            var user = User.withUsername("in28minutes")
                .password("{noop}dummy")
                .roles("USER")
                .build();

            
            var admin = User.withUsername("admin")
                    .password("{noop}dummy")
                    .roles("ADMIN")
                    .build();

            return new InMemoryUserDetailsManager(user, admin);
        }
    }
   ```

### DB에 저장하기
JDBC, JPA 로 자격증명을 검색합니다.

### LDAP (Lightweight Directory Access Protocol)
- 오픈 프로토콜을 씁니다.
