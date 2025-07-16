---
title: Spring boot Mybatis Mapper 작성에 대하여 알아보자.
date: 2025-07-16 21:30:00 +0900
categories: [Web, Backend]
tags: [backend, spring, mybatis, mapper, maven, xml, sql]
---

## **Mybatis란?**
Mybatis는 간단하게 설명하자면 Spring에 설정되어 있는 XML 파일 형태의 Mapper를 실제 SQL에 매핑시켜주는 프레임워크를 말한다.

JPA 처럼 자동으로 쿼리를 생성해준다거나 Entity를 매핑해주는 기능은 없으나, 쿼리에 직관성과 동적 쿼리 작성에 매우 특화되어있다.

## **Spring boot와 Mybatis**

1.`pom.xml` 파일에 Mybatis 의존성을 추가해준다.

```xml
<dependency>
  <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
		<version>3.0.4</version>
</dependency>
```

2.`application.properties`(`.yml`) 파일에 아래 내용을 추가한다.
```
mybatis.mapper-locations=classpath:mapper/*.xml
mybatis.type-aliases-package=com.libronote.domain
mybatis.configuration.map-underscore-to-camel-case=true
```

- `mybatis.mapper-locations`: Mybatis가 읽어올 XML 쿼리 파일 위치
- `mybatis.type-aliases-package`: Mapper에서 타입을 설정할 때 편리하도록 사용할 파라미터 또는 반환 객체 경로를 설정
- `mybatis.configuration.map-underscore-to-camel-case`: DB 컬럼명을 스네이크 케이스에서 자동으로 카멜 케이스로 변환하여 매핑하도록 설정

3.Mapper 인터페이스를 생성한다.
```java
@Mapper
public interface UserMapper {

    User getUserDetail(Long userSeq);
}
```

위 인터페이스는 Mapper XML 파일에 `namespace` 속성 값으로 지정할 때 사용하며, 선언된 메서드에 작성한 쿼리 내용이 적용된다.

4.Mapper XML 파일을 `application.properties`(`.yml`) 파일에 설정해둔 경로에 생성한다.
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.libronote.mapper.UserMapper">
</mapper>
```

- `mapper` 태그의 `namespace` 속성으로 연결할 Mapper 인터페이스 경로를 설정한다.

5.쿼리 작성

`INSERT` 쿼리 예시
```xml
<insert id="insertUser" parameterType="User" useGeneratedKeys="true" keyProperty="userSeq">
  INSERT INTO USER_INFO(email, password, nickname)
  VALUES(#{email}, #{password}, #{nickname})
</insert>
```

- `id`: 인터페이스에 선언한 메서드 이름과 일치해야함
- `parameterType`: 넘어가는 파라미터 값에 타입을 설정
- `useGeneratedKeys`: MySQL에 `AUTO_INCREMENT`와 같은 자동 생성되는 키값을 `keyProperty` 속성에 설정한 필드에 할당하도록 설정
- `keyProperty`: 자동 생성된 키값이 할당될 필드 설정

> `keyProperty`에 사용되는 필드명은 Java 객체의 필드명이여야한다.