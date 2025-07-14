---
title: Spring boot Filter를 이용한 로깅 설정에 대하여 알아보자
date: 2025-07-14 20:30:00 +0900
categories: [Web, Backend]
tags: [backend, spring, logging, filter, maven, servlet]
---

## **Filter VS Interceptor**
Spring boot를 사용하면서 로깅을 설정할 때 보통 `Filter`와 `Interceptor`에 로깅을 설정한다.

> Spring AOP를 사용하기도 한다.

이때, `Filter`와 `Interceptor`에 차이는 아래와 같다.

- `Filter`는 `Servlet Container` 수준에서 동작한다.
- `Interceptor`는 `DispatcherServlet`과 `Controller` 중간에 존재한다.

따라서, `Filter`는 요청과 응답에 로깅을 설정하고, `Interceptor`는 주로 인증/인가 처리, 컨트롤러 진입 전/후의 로직 처리, 로깅 등을 위해 사용된다.

## **Filter를 이용한 로깅 설정**
`servlet`에서 제공하는 `Filter` 인터페이스를 구현하여 처리하였다.

```java
@Component
@Slf4j
public class LoggingFilter implements Filter {

    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        HttpServletRequest request = (HttpServletRequest) servletRequest;
        HttpServletResponse response = (HttpServletResponse) servletResponse;

        long startedTime = System.currentTimeMillis();

        log.info("=================================================>>");
        log.info("URL : {} ",request.getRequestURL());
        log.info("HTTP_METHOD : {}", request.getMethod());
        log.info("URI : {}", request.getRequestURI());
        log.info("QUERY : {}", request.getQueryString());
        log.info("CONTENT_TYPE : {}", request.getContentType());
        log.info("=================================================>>");

        filterChain.doFilter(servletRequest, servletResponse);

        long endedTime = System.currentTimeMillis() - startedTime;

        log.info("<<=================================================");
        log.info("RESPONSE_ENDED_TIME : {}", endedTime);
        log.info("RESPONSE_STATUS : {}", response.getStatus());
        log.info("RESPONSE_CONTENT_TYPE : {}", response.getContentType());
        log.info("<<=================================================");
    }
}
```

`ServletRequest`, `ServletResponse`를 `HttpServletRequest`, `HttpServletResponse`로 형변환하여 사용하였다.

> `HttpServletRequest`와 `HttpServletResponse`는 각각 `ServletRequest`, `ServletResponse`의 하위 인터페이스로 HTTP 프로토콜에 초점이 맞춰져있다.

**구동 결과**

![loggingResult](/assets/img/filter_logging_result.png)