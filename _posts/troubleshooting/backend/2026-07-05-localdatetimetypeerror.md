---
"title: Troubleshooting - Type definition error: [simple type, class java.time.LocalDateTime] 에러 해결"
date: 2026-07-05 18:28:00 +0900
categories: [Troubleshooting]
tags: [mybatis, spring, java, maven, dependency, jackson, objectmapper]
---

## **에러**

Mybatis를 사용하여 테이블에서 데이터를 조회하고 등록일시와 수정일시를 응답으로 내려주던 도중 아래와 같은 에러가 발생함.

```log
org.springframework.http.converter.HttpMessageConversionException: Type definition error: [simple type, class java.time.LocalDateTime] at org.springframework.http.converter.json.AbstractJackson2HttpMessageConverter.writeInternal(AbstractJackson2HttpMessageConverter.java:491) at org.springframework.http.converter.AbstractGenericHttpMessageConverter.write(AbstractGenericHttpMessageConverter.java:124) at org.springframework.web.servlet.mvc.method.annotation.AbstractMessageConverterMethodProcessor.writeWithMessageConverters(AbstractMessageConverterMethodProcessor.java:344) at org.springframework.web.servlet.mvc.method.annotation.HttpEntityMethodProcessor.handleReturnValue(HttpEntityMethodProcessor.java:263)
```

## **원인**

Spring이 응답 객체를 `JSON`으로 변환하는 과정에서 `ObjectMapper`가 `LocalDateTime`을 처리하지 못해 예외가 발생함.

Spring은 기본적으로 `JavaTimeModule`이 등록되어 있는 `ObjectMapper`를 사용하여 직렬화 및 역직렬화를 처리하지만, 현재 프로젝트에서는 `ObjectMapper`를 직접 `Bean`으로 등록해주고 있기 때문에 문제가 발생하게 됨.

`ObjectMapperConfig.java`
```java
@Configuration
public class ObjectMapperConfig {

    @Bean
    public ObjectMapper objectMapper() {
        return new ObjectMapper();
    }
}
```

## **해결**

`JavaTimeModule`을 등록하여 `LocalDateTime`을 처리할 수 있도록 설정하고, `WRITE_DATES_AS_TIMESTAMPS` 옵션을 비활성화하여 날짜를 ISO-8601 문자열 형식으로 직렬화하도록 변경하였음.

`ObjectMapperConfig.java`
```java
@Configuration
public class ObjectMapperConfig {

    @Bean
    public ObjectMapper objectMapper() {
        ObjectMapper objectMapper = new ObjectMapper();
        objectMapper.registerModule(new JavaTimeModule());
        objectMapper.disable(SerializationFeature.WRITE_DATES_AS_TIMESTAMPS);

        return objectMapper;
    }
}
```

## **Reference**
- [https://jnjeaaaat.tistory.com/79](https://jnjeaaaat.tistory.com/79)