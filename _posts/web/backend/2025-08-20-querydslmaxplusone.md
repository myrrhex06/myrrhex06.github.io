---
title: "QueryDSL Max + 1 구현에 대해 알아보자."
date: 2025-08-20 21:30:00 +0900
categories: [Web, Backend]
tags: [backend, spring, querydsl, sql]
---

QueryDSL은 JPA에 단점 중 하나인 동적 쿼리 작성이 힘들다는 점을 보완하기 위해 주로 사용하는 라이브러리이다.

## **Max + 1 구현**
구현 예시
```java
jpaQueryFactory
    .select(qExampleEntity.id.max()
    .coalesce(0L).add(1L))
    .from(qExampleEntity)
    .fetchOne();
```

- `max()` 메서드: 해당 컬럼에 최대값을 가져옴
- `coalesce()` 메서드: A가 `NULL` 값이면 B를 반환하는 메서드
- `add()` 메서드: `+` 연산

위와 같이 `@Query` 어노테이션을 사용하여 Native Query를 작성하지 않아도 간편하게 구현이 가능하다.

> `@Query` 어노테이션을 사용하게 되면 SQL문을 하드코딩으로 일일이 넣어줘야하며, DB가 바뀌었을 경우 호환성 문제도 생각해야하기 때문에 동적 쿼리 작성에는 QueryDSL이 더 적합하다.
{: .prompt-tip }