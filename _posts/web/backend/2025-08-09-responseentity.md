---
title: "ResponseEntity에 대해 알아보자."
date: 2025-08-09 19:00:00 +0900
categories: [Web, Backend]
tags: [backend, spring, restful, http, response]
---

## **ResponseEntity**
`Controller`에서 정의한 엔드포인트의 Http 응답을 세밀하게 조작할 수 있게 해주는 클래스이다.

이 클래스를 통해 Http 헤더, 바디, 상태 코드까지 조작이 가능하다.

`ResponseEntity` 내부 코드
```java
public class ResponseEntity<T> extends HttpEntity<T> {
    private final Object status;
		
		public ResponseEntity(HttpStatus status) {
        this((Object)null, (MultiValueMap)null, (HttpStatus)status);
    }

    public ResponseEntity(@Nullable T body, HttpStatus status) {
        this(body, (MultiValueMap)null, (HttpStatus)status);
    }

    public ResponseEntity(MultiValueMap<String, String> headers, HttpStatus status) {
        this((Object)null, headers, (HttpStatus)status);
    }

    public ResponseEntity(@Nullable T body, @Nullable MultiValueMap<String, String> headers, HttpStatus status) {
        this(body, headers, (Object)status);
    }

    public ResponseEntity(@Nullable T body, @Nullable MultiValueMap<String, String> headers, int rawStatus) {
        this(body, headers, (Object)rawStatus);
    }
    
    ...
}
```

`ResponseEntity`는 `HttpEntity`라는 클래스를 상속받고 있다는 것을 확인할 수 있다.

`HttpEntity`는 Http 요청이나 응답의 헤더, 바디 정보를 하나의 객체로 표현한 클래스이다.

`HttpEntity` 내부 코드
```java
public class HttpEntity<T> {
    public static final HttpEntity<?> EMPTY = new HttpEntity();
    private final HttpHeaders headers;
    @Nullable
    private final T body;

    protected HttpEntity() {
        this((Object)null, (MultiValueMap)null);
    }

    public HttpEntity(T body) {
        this(body, (MultiValueMap)null);
    }

    public HttpEntity(MultiValueMap<String, String> headers) {
        this((Object)null, headers);
    }

    public HttpEntity(@Nullable T body, @Nullable MultiValueMap<String, String> headers) {
        this.body = body;
        this.headers = HttpHeaders.readOnlyHttpHeaders((MultiValueMap)(headers != null ? headers : new HttpHeaders()));
    }

  ...
}
```

두 클래스의 차이점은 `HttpEntity`는 Http header, body 정보만 다룰 수 있지만, `ResponseEntity`는 Http header, body, 상태 코드까지 조작이 가능하다.

또한, `ResponseEntity`가 제공하는 생성자의 종류도 다양하다.

```java
public ResponseEntity(HttpStatus status) {
    this((Object)null, (MultiValueMap)null, (HttpStatus)status);
}
```

- Http 상태 코드 설정 생성자

```java
public ResponseEntity(@Nullable T body, HttpStatus status) {
    this(body, (MultiValueMap)null, (HttpStatus)status);
}
```

- Http body, 상태 코드 설정 생성자

```java
public ResponseEntity(MultiValueMap<String, String> headers, HttpStatus status) {
    this((Object)null, headers, (HttpStatus)status);
}
```

- Http header, 상태 코드 설정 생성자

```java
public ResponseEntity(@Nullable T body, @Nullable MultiValueMap<String, String> headers, HttpStatus status) {
    this(body, headers, (Object)status);
}
```

- Http body, header, 상태 코드 설정 생성자

```java
public ResponseEntity(@Nullable T body, @Nullable MultiValueMap<String, String> headers, int rawStatus) {
    this(body, headers, (Object)rawStatus);
}
```

- Http body, header, 상태 코드(정수값으로 사용) 설정 생성자

생성자를 사용하는 것 말고도 상태 코드에 따른 `Builder`도 제공해준다.

Builder 사용의 장점은 아래와 같다.
- 체이닝으로 가독성이 높아짐
- 불필요한 `new` 연산 제거

`ResponseEntity` 제공 `Builder`
```java
public static BodyBuilder status(int status) {
    return new DefaultBuilder(status);
}

public static BodyBuilder ok() {
    return status(HttpStatus.OK);
}

public static <T> ResponseEntity<T> ok(@Nullable T body){
    return ok().body(body);
}

public static BodyBuilder internalServerError() {
    return status(HttpStatus.INTERNAL_SERVER_ERROR);
}

....
```

이 `Builder`들 외에도 다양한 `Builder`들을 제공해준다.

사용 예시
```java
@RestController
public class ExampleController {
    
    @GetMapping("/constructor")
    public ResponseEntity<String> entityExample() {
        return new ResponseEntity<>("Hello World", HttpStatus.OK);
    }

    @GetMapping("/builder")
    public ResponseEntity<String> builderExample() {
        return ResponseEntity.ok("Hello World");
    }
}
```

이렇게 `ResponseEntity`를 사용하면, 응답 객체에 세밀한 설정을 수월하게 진행할 수 있다.