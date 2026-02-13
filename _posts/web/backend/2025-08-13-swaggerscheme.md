---
title: "Backend - Spring boot swagger UI JWT 인증 적용 방법에 대해 알아보자."
date: 2025-08-13 20:45:00 +0900
categories: [Web, Backend]
tags: [backend, spring, swagger, jwt, auth, security, scheme]
---

## **Swagger UI JWT 토큰 인증 방식 적용 방법**
Swagger UI에 JWT 인증 방식을 적용시키는 방법은 `@SecurityScheme`, `@SecurityRequirement`를 사용하는 것이다.

### **@SecurityScheme**
`SwaggerConfig.java` 예시
```java
@SecurityScheme(
        name = "Jwt Auth",
        type = SecuritySchemeType.HTTP,
        bearerFormat = "JWT",
        scheme = "bearer"
)

public class SwaggerConfig {
  ...
}
```

- `name`: Swagger UI에서 인증 스킴명
    - `@SecurityRequirement` 어노테이션을 사용해서 인증 스킴을 지정할 때 `name` 값으로 사용됨
- `type`: 인증 유형을 지정
    - `SecuritySchemeType.HTTP`: HTTP 프로토콜 기반 인증 방식을 뜻함
    - 이 외에도 `APIKEY`, `OAUTH2`등이 있음
- `bearerFormat`: 인증 스킴에서 사용되는 토큰 형식을 알려줌
    - 단순 설명 용도로 사용되며 실질적인 기능은 없음
- `scheme`: 인증 스킴의 이름
    - `bearer`: `Authorization: Bearer <token>` 형태로 요청을 날려줌
    - `basic`: `Authorization: Basic <credentials>` 형태로 요청을 날려줌

### **@SecurityRequirement**
해당 어노테이션에 `name` 속성을 사용하여 정의한 `@SecurityScheme`와 연결한다.

`@SecurityRequirement` 사용 예시
```java
@GetMapping("/example")
@PreAuthorize("hasRole('ROLE_USER')")
@SecurityRequirement(name="Jwt Auth")
public String example(){
	return "Hello";
}
```