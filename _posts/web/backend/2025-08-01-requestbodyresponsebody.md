---
title: "@RequestBody, @ResponseBody에 대해서 알아보자."
date: 2025-08-01 21:00:00 +0900
categories: [Web, Backend]
tags: [backend, spring]
---

Spring boot를 사용하면서 RESTful API 서버를 개발하는데 사용되는 `@RequestBody`, `@ResponseBody`에 대해서 알아보자.

## **@RequestBody**
`@RequestBody` 어노테이션은 HTTP 요청 본문(body)에 있는 데이터를 Java 객체로 역직렬화 시킬 때 사용한다.

`@RequestBody.java`
```java
@Target({ElementType.PARAMETER})
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface RequestBody {
    boolean required() default true;
}
```

위 코드에서 확인할 수 있듯이 `@RequestBody`는 파라미터에만 적용이 가능하다.

**처리 흐름**
1. 클라이언트가 `JSON` 데이터로 HTTP 요청을 보냄
2. `@RequestBody` 어노테이션이 있을 경우 `HttpMessageConverter`가 동작하여 `JSON`을 Java 객체로 역직렬화 처리
3. 컨트롤러 메서드에서 파라미터로 활용

## **@ResponseBody**
컨트롤러에서 반환한 객체를 JSON 같은 응답 본문으로 직렬화 시킬 때 사용한다.

`ResponseBody.java`
```java
@Target({ElementType.TYPE, ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface ResponseBody {
}
```

위 코드에서 확인할 수 있듯이 `@ResponseBody` 어노테이션은 `@RequestBody` 어노테이션과는 다르게 클래스와 메서드에만 적용이 가능하다.

> `@ResponseBody` 어노테이션을 붙이지 않을 경우 기본적으로 `ViewResolver`가 동작하여 템플릿 파일을 찾게된다.
{: .prompt-tip }

**처리 흐름**

1. 메서드가 객체를 리턴
2. `HttpMessageConverter`가 동작하여 Java 객체를 `JSON` 형태로 직렬화

### **@RestController**
`@RestController` 어노테이션은 아래와 같이 구현되어있다.

`RestController.java`
```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Controller
@ResponseBody
public @interface RestController {
    @AliasFor(
        annotation = Controller.class
    )
    String value() default "";
}
```

위 코드를 확인해보면 `@Controller` 어노테이션과 `@ResponseBody` 어노테이션이 같이 붙어있으며, 클래스 레벨에만 적용시킬 수 있다.

즉, `@RestController` 어노테이션은 클래스 단위로 붙기 때문에 이 어노테이션이 붙은 클래스의 모든 메서드들의 반환 값은 `@ResponseBody` 어노테이션 덕분에 자동으로 직렬화되서 리턴된다.

**처리 흐름**

1. 컨트롤러 메서드가 객체를 리턴
2. `@ResponseBody`가 자동으로 적용되어 있음
3. `HttpMessageConverter`가 `JSON`으로 변환
4. 응답 본문에 `JSON`이 담김