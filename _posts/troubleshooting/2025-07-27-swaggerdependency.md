---
title: Handler dispatch failed java.lang.NoSuchMethodError 에러 해결
date: 2025-07-27 07:30:00 +0900
categories: [Troubleshooting]
tags: [swagger, spring, java, maven, dependency]
---

## **에러**
`ExceptionHandler` 설정 후 swagger를 접속하자 아래와 같은 화면이 나타났다.

![swaggerError](/assets/img/swagger_error.png)

에러 메시지

```java
Handler dispatch failed: java.lang.NoSuchMethodError: 'void org.springframework.web.method.ControllerAdviceBean.<init>(java.lang.Object)'
```

## **원인**
현재 사용중이던 swagger 버전은 아래와 같다.

```xml
<dependency>
  <groupId>org.springdoc</groupId>
  <artifactId>springdoc-openapi-starter-webmvc-ui</artifactId>
  <version>2.1.0</version>
</dependency>
```

Spring boot 버전은 3.4.7 버전으로 위 2.1.0 버전은 서로 호환되지 않아 발생한 문제이다.

## **해결**
swagger 의존성을 2.8.8 버전으로 설정하여 해결하였다.

```xml
<dependency>
  <groupId>org.springdoc</groupId>
  <artifactId>springdoc-openapi-starter-webmvc-ui</artifactId>
  <version>2.8.8</version>
</dependency>
```

