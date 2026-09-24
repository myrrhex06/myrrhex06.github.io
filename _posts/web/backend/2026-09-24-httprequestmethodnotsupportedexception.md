---
title: "Spring Boot - HttpRequestMethodNotSupportedException 핸들링"
date: 2026-09-24 22:10:00 +0900
categories: [Web, Backend]
tags: [spring, springboot, exception, handler]
---

```java
@RestControllerAdvice
public class GlobalExceptionHandler extends ResponseEntityExceptionHandler {

    @Override
    protected ResponseEntity<Object> handleHttpRequestMethodNotSupported(HttpRequestMethodNotSupportedException ex, HttpHeaders headers, HttpStatus status, WebRequest request) {
        return handleExceptionInternal(ex,
                GenericResponse.getErrorBody(HttpStatus.BAD_REQUEST, ex.getMessage()),
                new HttpHeaders(), HttpStatus.BAD_REQUEST, request);
    }
}
```

- `ResponseEntityExcepitonHandler` 클래스를 상속받은 후 `handleHttpRequestMethodNotSupported` 메서드를 오버라이딩해서 처리함.