---
title: "다중 컬럼 서브쿼리란?"
date: 2025-09-30 20:20:00 +0900
categories: [Language, SQL]
tags: [sql, mysql, subquery, select]
---

## **다중 컬럼 서브쿼리란?**
- `SELECT` 서브쿼리에 두 개 이상의 컬럼이 포함되는 것을 의미함.
  - `WHERE` 절에서 여러 컬럼을 동시에 비교할 때 유용함.

예시 1
```sql
SELECT *
FROM users u
WHERE (u.name, u.email) = (SELECT name, email FROM users WHERE user_id = 2);
```

예시 2
```sql
SELECT *
FROM users u
WHERE (u.name, u.address) IN (SELECT name, address FROM users WHERE address LIKE '서울%');
```