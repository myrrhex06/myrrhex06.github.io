---
title: "상관 서브쿼리란?"
date: 2025-10-06 18:20:00 +0900
categories: [Language, SQL]
tags: [sql, mysql, subquery, select]
---

## **상관 서브쿼리란?**
- 메인쿼리에서 처리 중인 행의 특정 값을 알아야만 계산을 수행할 수 있을 때 사용함.
  - 메인쿼리와 서브쿼리가 의존성을 가지고 동작하는 것임.

> 비상관 서브쿼리는 서브쿼리가 한 번 실행된 후 그 결과값을 메인쿼리가 사용하는 것을 말함.
{: .prompt-tip }

예시
```sql
SELECT *
FROM articles AS a1
WHERE views >= (
    SELECT AVG(a2.views)
    FROM articles AS a2
    WHERE a2.category = a1.category
);
```

**상관 서브쿼리의 동작 방식**

1. 메인 쿼리가 먼저 한 행(row)을 읽음.
2. 읽혀진 행의 값을 서브쿼리에 전달한 후 서브쿼리가 실행됨
3. 서브쿼리 결과를 통해 메인쿼리의 `WHERE` 조건을 판단함

이때, 상관 서브쿼리는 서브쿼리가 메인쿼리의 행 수만큼 반복 실행될 수 있음.

**상관 서브쿼리의 성능**
- 메인쿼리의 행 수가 많을 경우 성능이 매우 떨어질 수 있음
  - 보통 상관 서브쿼리는 `JOIN`으로 처리가 가능함.
  - DB 옵티마이저가 `JOIN`을 더 효율적으로 처리해줌