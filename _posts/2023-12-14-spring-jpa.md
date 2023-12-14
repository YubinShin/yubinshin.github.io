---
title: 스프링 부트로 배우는 JPA
date: 2023-12-14
categories: [blog, java]
tags: [java, spring_boot, orm]
---


## Hibernate와 JPA의 차이

### JPA (Jakarta Persistence API)
JPA는 Java의 표준 **ORM** (Object-Relational Mapping) 명세입니다. 이는 객체와 관계형 데이터베이스 테이블 간의 매핑을 쉽게 해주는 규칙과 API를 제공합니다.

JPA 어노테이션들 
`@Entity`, `@Column` 등은 JPA가 제공하는 어노테이션으로, 엔터티의 속성과 데이터베이스 컬럼 간의 매핑을 정의합니다. `@EntityManager`는 JPA의 핵심 클래스 중 하나로, 엔터티의 생명 주기를 관리합니다.

### Hibernate
Hibernate는 JPA 명세를 구현한 가장 널리 사용되는 프레임워크 중 하나입니다. JPA는 **인터페이스만 제공**하며, 실제 작동은 Hibernate와 같은 구현체가 담당합니다.

TopLink, EclipseLink와 같은 다른 JPA 구현체들도 있습니다. **JPA 어노테이션**을 사용하면, 필요에 따라 다른 구현체로 **쉽게 전환**할 수 있습니다.

> Hibernate의 인기: Hibernate는 강력한 캐싱, 지연 로딩, 연관 관계 매핑, 성능, 안정성 등의 특징 때문에 인기가 많습니다. 또한, 활발한 커뮤니티와 지속적인 개선도 인기 요인 중 하나입니다.

> Mongoose: JavaScript와 함께 사용되며, MongoDB(NoSQL 데이터베이스)를 위한 **ODM** (Object-Document Mapping) 라이브러리입니다. Mongoose는 MongoDB의 문서를 JavaScript 객체로 매핑합니다.

> Prisma: Node.js와 TypeScript에서 사용되며, 여러 종류의 데이터베이스(관계형 및 일부 NoSQL)를 위한 **ORM**입니다. Prisma는 데이터베이스 스키마를 기반으로 타입 안전한 데이터 액세스를 제공합니다.

## JPA 와 Hibernate 를 사용하면 

1. JpaRepository<엔티티명, pk_type> 라는 인터페이스를 extends 해서 crud 를 아주 쉽게 할 수 있다. 데이터 접근 계층의 메서드를 별도로 구현하지 않아도 된다.
2. Jpa 와 Hibernate를 사용하면 매우 쉽게 설정만 바꿔주면 데이터베이스를 이사할 수 있다.

    예시 ) [https://github.com/YubinShin/dispring-be/commit/04cae438b094d73eeed4434fff126fac37ab0758](https://github.com/YubinShin/dispring-be/commit/04cae438b094d73eeed4434fff126fac37ab0758)

    `spring.jpa.hibernate.ddl-auto=update`

    H2 같은 인메모리 데이터베이스와 연결할 때는 Spring Boot Auto-configuration이 직접 엔터티를 살펴보고 테이블을 생성합니다.
    하지만 MySQL 같은 데이터베이스로 연결하는 경우에는 Spring Boot Auto-configuration이 테이블을 생성하지 않아요.
    Spring Boot Auto-configuration이 모든 테이블을 생성해주도록 하려고 합니다.

    `spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQLDialect`
    Hibernate가 db마다 적절한 sql 문법을 생성하게 한다.

Node.js 보다 Spring 이 인기 많은 이유를 배우면서 느끼고 있다. 강력한 기능이 굉장히 많다.