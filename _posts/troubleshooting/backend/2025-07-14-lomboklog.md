---
title: Troubleshooting - java:cannot find symbol 에러 해결
date: 2025-07-14 20:30:00 +0900
categories: [Troubleshooting]
tags: [lombok, spring, java, maven]
---

## **에러**
`Filter` 인터페이스를 구현하여 로그 설정 중 아래와 같은 에러가 발생했다.
![lombokError](/assets/img/lombok_slf4j_error.png)

## **원인**
`pom.xml`에 `lombok` 의존성만 추가되어있고 플러그인이 제대로 추가되있지 않아서 발생한 것이였다.

## **해결**
`pom.xml`에 아래 플러그인을 추가해서 해결하였다.
```xml
<plugin>
  <groupId>org.apache.maven.plugins</groupId>
  <artifactId>maven-compiler-plugin</artifactId>
  <configuration>
    <annotationProcessorPaths>
      <path>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <version>사용하는_Lombok_버전</version>
      </path>
    </annotationProcessorPaths>
  </configuration>
</plugin>
```