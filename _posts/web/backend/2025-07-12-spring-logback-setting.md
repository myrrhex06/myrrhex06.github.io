---
title: Spring boot logback 설정에 대하여 알아보자.
date: 2025-07-12 08:20:00 +0900
categories: [Web, Backend]
tags: [backend, spring, logging, logback, xml]
---

## **`System.out.println`을 로깅에 사용하지 않는 이유**

Java를 처음 배울 때 사용하는 `System.out.println`을 로깅 작업에는 사용하지 않는다.<br>
그 이유는 아래와 같다.

**1. 레벨 관리**<br>
`logger`를 사용하면 로그 레벨(`INFO`, `DEBUG`, `ERROR` 등)에 맞게 중요도에 따라 필터링을 하기 수월하지만, `System.out.println`은 그런 기능이 없다.

**2. 추적 및 분석**<br>
`logger`는 기본적으로 메타 정보(타임스탬프, 쓰레드명, 클래스명 등)을 붙여주고 커스텀하더라도 붙이기가 매우 쉽다.<br>
반면 `System.out.println`에 메타 정보를 붙이려면 상당히 귀찮은 작업들이 생긴다. 

**3. 포맷팅과 성능**<br>
`logger`는 포맷 지정, 변수 치환을 효율적으로 처리해 불필요한 연산을 최소화하지만, `System.out.println`은 매번 문자열을 다 만들고 출력해서 성능이 `logger`에 비해 느리다.

**4. 운영 환경 대응**<br>
운영 환경에서는 `DEBUG` 로그를 제거하고 INFO 로그 이상만 보는게 일반적이다.<br>
`System.out.println`은 이런 제어가 불가능하다.

## **logback이란?**
Spring boot에서 기본 제공하는 로깅 프레임워크로, Slf4j 인터페이스를 구현한 고성능 로깅 프레임워크이다.

> logback말고도 log4j, log4j2가 존재한다.
{: .prompt-tip }

### **Spring boot logback 설정 방법**

Spring boot는 기본적으로 logback 의존성을 가지고 있기 때문에 의존성을 추가로 설정해 줄 필요는 없다.

우선 `resource` 패키지 아래에 `logback-spring.xml` 파일을 생성해준다.

> `application.yml` 또는 `application.property` 파일에서도 설정이 가능하지만, 세밀한 설정을 위해서는 `logback-spring.xml` 파일이 필요하다.
{: .prompt-tip }

먼저 예시를 보고 설명하겠다.

`logback-spring.xml`
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<configuration>

    <property name="PATTERN_VALUE" value="%boldGreen([%d{yyyy-MM-dd HH:mm:ss.SSS}]) %highlight([%-5level]) %magenta([%thread]) %logger %msg%n"/>
    <property name="ROLLING_FILE_PATH" value="logs/debug_%d{yyyy-MM-dd}.log" />
    <property name="FILE_NAME" value="logs/debug.log"/>

    <appender name="CONSOLE" class="ch.qos.logback.core.ConsoleAppender">
        <filter class="ch.qos.logback.classic.filter.ThresholdFilter">
            <level>DEBUG</level>
        </filter>
        <encoder>
            <pattern>${PATTERN_VALUE}</pattern>
        </encoder>
    </appender>

    <appender name="FILE" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <file>${FILE_NAME}</file>
        <append>true</append>
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <fileNamePattern>
                ${ROLLING_FILE_PATH}
            </fileNamePattern>
            <maxHistory>
                30
            </maxHistory>
        </rollingPolicy>
        <encoder>
            <pattern>[%d{yyyy-MM-dd HH:mm:ss.SSS}] [%-5level] [%thread] %logger %msg%n</pattern>
        </encoder>
    </appender>

    <root level="INFO">
        <appender-ref ref="CONSOLE"/>
        <appender-ref ref="FILE"/>
    </root>
</configuration>
```

위 파일은 Spring boot 애플리케이션이 구동될 때 찍히는 로그에 대한 설정 정보를 담고 있다.

- `configuration`: logback 설정을 위한 최상위 태그
- `property`: 설정 파일에서 사용할 상수 정의
- `appender`: 로그를 어디에 기록할지 식별
- `filter`: 로그를 출력할 때 조건을 설정
- `pattern`: 출력할 로그 패턴 설정
- `file`: 기본적으로 로그가 기록되는 파일 이름 설정 (롤링 전 기본 로그 파일)
- `rollingPolicy`: 로그 파일 롤링 정책 설정
- `fileNamePattern`: 로그 파일이 롤링될 때 설정되는 이름
- `maxHistory`: 최대 저장 기간
- `root`: `root logger` 설정
- `appender-ref`: `root logger`에 연결할 `appender` 설정

> 롤링이란 로그 파일을 특정 조건에 맞춰 새 로그 파일로 갈아끼우는 작업을 뜻한다.
{: .prompt-tip }

각 조건에 설정된 클래스들은 아래와 같다.

- `ConsoleAppender`: 로그를 콘솔에 찍음
- `RollingFileAppender`: 로그를 파일에 기록하면서 특정 조건이 되면 롤링
- `ThresholdFilter`: 지정한 로그 레벨 이상만 출력
- `TimeBasedRollingPolicy`: 설정한 기간을 기준으로 로그 파일을 롤링

