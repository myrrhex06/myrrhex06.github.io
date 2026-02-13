---
title: Backend - Spring boot @Scheduled cron 표현식에 대해 알아보자.
date: 2025-07-31 22:00:00 +0900
categories: [Web, Backend]
tags: [backend, spring, cron, scheduler]
---

오늘은 Spring에서 `@Scheduled` 어노테이션을 사용할 때 주로 사용하는 `cron` 표현식에 대해서 알아보자.

들어가기에 앞서 `@Scheduled`에 대해 간략하게 알아보자.

## `@Scheduled`란?
`@Scheduled`는 Spring에서 스케줄링을 설정할 때 사용하는 어노테이션으로, 설정된 시간마다 반복적으로 호출되는 로직을 정의한다.

주로 통계 데이터를 수집, 알림 발송 등 반복적인 작업에 사용된다.

`cron`은 `@Scheduled`의 동작 시점을 정의하는 방법 중 하나이다.

### `cron` 표현식 구조
`cron` 표현식의 구조는 아래와 같다.

```
초 분 시 일 월 요일 연도(옵션)
```

> `@Scheduled` 에서는 주로 6자리까지만 사용한다.(연도를 넣지 않음)
{: .prompt-tip }

사용 예시 1

```java
@Scheduled(cron = "0 0 1 * * ?") 
```

- `?`: 무시
- `*`: 전체

위 예시대로 설정했을 경우 스케줄링 작업은 매월, 매일 새벽 1시에 스케줄링 작업이 실행된다.

> `?`는 일이나 요일 자리에만 사용 가능하다.<br>
둘 중 하나를 명확히 지정했으면, 나머지 하나에는 `?`로 “무시”를 명시해야 한다.
{: .prompt-tip }

사용 예시 2
```java
@Scheduled(cron = "0 */10 * * * ?")
```

- `*/n`: 0부터 n 단위로 반복

위 예시대로 설정했을 경우, 매시간마다 10분 간격(0분, 10분, 20분...)으로 스케줄링 작업이 실행된다.

> `@Scheduled`는 `cron` 외에도 `fixedRate`, `fixedDelay` 방식으로도 주기 설정이 가능하다.
{: .prompt-tip }