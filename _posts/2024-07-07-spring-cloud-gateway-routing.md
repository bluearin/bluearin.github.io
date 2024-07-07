---
title: Spring Cloud Gateway - Routing 기능
author: bluearin
date: 2024-07-07 15:20:00 +0900
categories: [Dev, Java]
tags: [dev,java,spring,gateway]
---

오늘은 Spring Cloud Gateway의 핵심 기능 중 하나인 라우팅에 대해 알아보려고 합니다. 마이크로서비스 아키텍처를 구현할 때 API Gateway는 필수적인 컴포넌트인데요, 그 중에서도 Spring Cloud Gateway는 Java 개발자들 사이에서 인기 있는 선택지입니다. 특히 라우팅 기능은 Gateway의 꽃이라고 할 수 있습니다.

## 라우팅이란 무엇인가요?

간단히 말해, 라우팅은 클라이언트의 요청을 적절한 서비스로 전달하는 과정입니다. 마치 우체부가 편지를 올바른 주소지로 배달하는 것과 비슷하다고 생각하면 됩니다.

## Spring Cloud Gateway의 라우팅 구성 요소

Spring Cloud Gateway의 라우팅은 크게 세 가지 요소로 구성됩니다:

1. Route: 라우팅의 기본 단위로, 목적지 URI, 조건자(Predicate) 목록, 필터 목록으로 구성됩니다.
2. Predicate: 요청이 라우트와 매칭되는지 결정하는 조건입니다.
3. Filter: 요청이나 응답을 수정할 수 있는 컴포넌트입니다.

## 라우팅 설정하기

Spring Cloud Gateway에서 라우팅을 설정하는 방법은 크게 두 가지가 있습니다:

### 1. Java DSL 사용

```java
@Configuration
public class GatewayConfig {

    @Bean
    public RouteLocator customRouteLocator(RouteLocatorBuilder builder) {
        return builder.routes()
            .route("user_service_route", r -> r
                .path("/users/**")
                .uri("lb://user-service"))
            .route("order_service_route", r -> r
                .path("/orders/**")
                .uri("lb://order-service"))
            .build();
    }
}
```

### 2. application.yml 사용

```yml
spring:
  cloud:
    gateway:
      routes:
        - id: user_service_route
          uri: lb://user-service
          predicates:
            - Path=/users/**
        - id: order_service_route
          uri: lb://order-service
          predicates:
            - Path=/orders/**
```

## 실용적인 라우팅 예제

자, 이제 실제로 자주 사용되는 라우팅 설정 몇 가지를 살펴볼까요?

### 1. 경로 기반 라우팅

```yml
spring:
  cloud:
    gateway:
      routes:
        - id: product_route
          uri: lb://product-service
          predicates:
            - Path=/api/products/**
```

이 설정은 /api/products로 시작하는 모든 요청을 product-service로 라우팅합니다.

### 2. 메소드 기반 라우팅

```yml
spring:
  cloud:
    gateway:
      routes:
        - id: post_route
          uri: lb://post-service
          predicates:
            - Path=/api/posts
            - Method=POST
```

이 설정은 /api/posts로 오는 POST 요청만을 post-service로 라우팅합니다.

### 3. 헤더 기반 라우팅

```yml
spring:
  cloud:
    gateway:
      routes:
        - id: admin_route
          uri: lb://admin-service
          predicates:
            - Path=/api/admin/**
            - Header=X-Role, admin
```

이 설정은 /api/admin으로 오는 요청 중 'X-Role' 헤더가 'admin'인 경우에만 admin-service로 라우팅합니다.

## 마치며

Spring Cloud Gateway의 라우팅 기능은 정말 강력하고 유연합니다. 이번 글에서 다룬 내용은 빙산의 일각에 불과하니, 여러분도 직접 다양한 설정을 시도해보시기 바랍니다.