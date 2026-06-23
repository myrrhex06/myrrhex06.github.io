---
title: "Backend - Spring Security AuthenticationEntryPoint란?"
date: 2026-06-22 06:35:00 +0900
categories: [Web, Backend]
tags: [spring, security, exceptionhandler]
---

## **Spring Security 예외 처리 구조**

Spring Security의 예외 처리 구조는 아래와 같음.

<img src="/assets/img/spring-security-exception-handler-architecture.png" alt="image" width="500">

> 출처: [Spring Security 공식 문서](https://docs.spring.io/spring-security/reference/servlet/architecture.html)

1. `ExceptionTranslationFilter`는 `FilterChain.doFilter(request, response)`를 호출함.
2. 만약 사용자가 인증되지 않았거나, `AuthenticationException`이 발생했다면 인증 절차를 시작함.
   1. `SecurityContextHolder`를 비움.
   2. `HttpServletRequest`를 저장해서 인증 성공 시 다시 원래 요청을 보낼 수 있도록함.
   3. `AuthenticationEntryPoint`는 클라이언트에게 자격 증명을 요청하기 위해 사용함. (ex. 로그인 페이지로 리다이렉션)
3. `AccessDeniedException`이 발생할 경우에는 `AccessDeniedHandler`가 호출됨.

> 만약 `AccessDeniedException`, `AuthenticationException` 예외가 발생하지 않는다면, `ExceptionTranslationFilter`는 아무것도 하지 않음.
{: .prompt-tip }

## **AuthenticationEntryPoint 인터페이스**

`AuthenticationEntryPoint.java`
```java
public interface AuthenticationEntryPoint {
    void commence(HttpServletRequest request, HttpServletResponse response, AuthenticationException authException) throws IOException, ServletException;
}
```

`commence` 메서드는 인증되지 않은 사용자가 요청을 보내 `AuthenticationException`이 발생했을 경우에 호출된느 메서드임.

클라이언트에게 인증을 요청하는 동작을 수행함.

이 메서드를 구현하여 로그인 페이지로 리다이렉트 시키거나, json 형태의 에러를 내려보낼수도 있음.

## **실제 사용 예시**

현재 RESTful API를 기반으로 개발을하고 있기 때문에 응답으로는 리다이렉트 대신 json 형태의 에러를 내려주도록 구현함.

`CustomAuthenticationEntryPoint.java`
```java
@Component
@Slf4j
@RequiredArgsConstructor
public class CustomAuthenticationEntryPoint implements AuthenticationEntryPoint {

    private final ObjectMapper objectMapper;

    @Override
    public void commence(HttpServletRequest request, HttpServletResponse response, AuthenticationException authException) throws IOException, ServletException {
        log.error("Unauthenticated Request : ", authException);

        ResponseWrapper responseWrapper = ResponseWrapper.builder()
                .status(HttpStatus.UNAUTHORIZED.value())
                .message("인증되지 않은 사용자입니다.")
                .result(null)
                .build();

        String responseBody = objectMapper.writeValueAsString(responseWrapper);

        response.setContentType(MediaType.APPLICATION_JSON_VALUE);
        response.setStatus(HttpStatus.UNAUTHORIZED.value());
        response.setCharacterEncoding("UTF-8");
        response.getWriter().write(responseBody);
    }
}
```

## **참고**
- [https://docs.spring.io/spring-security/reference/servlet/architecture.html](https://docs.spring.io/spring-security/reference/servlet/architecture.html)
- [https://yoo-dev.tistory.com/28](https://yoo-dev.tistory.com/28)
- [https://velog.io/@superkkj/AuthenticationEntryPoint-%EC%9D%B4%ED%95%B4](https://velog.io/@superkkj/AuthenticationEntryPoint-%EC%9D%B4%ED%95%B4)