---
title: "Backend - Spring Boot WAR 톰켓 설정 방법"
date: 2025-09-29 17:50:00 +0900
categories: [Web, Backend]
tags: [backend, springboot, tomcat, war]
---

1.`pom.xml`에서 내장 톰켓 의존성 제거
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
    <!-- 톰캣 제거 -->
    <exclusions>
        <exclusion>
            <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-tomcat</artifactId>
        </exclusion>
    </exclusions>
</dependency>
```

2.`Application` 클래스에서 `SpringBootServletInitializer` 클래스를 상속받아 `configure` 메서드 구현
```java
@SpringBootApplication
public class UsersampleApplication extends SpringBootServletInitializer {

	@Override
	protected SpringApplicationBuilder configure(SpringApplicationBuilder builder) {
		return builder.sources(UsersampleApplication.class);
	}
}
```