---
title: "SQL - 스칼라 서브쿼리란?"
date: 2025-09-27 15:10:00 +0900
categories: [Language, SQL]
tags: [sql, mysql, subquery, select, scala]
---

## **스칼라 서브쿼리란?**
- 단일 행(row), 단일 열(column)을 반환하는 서브쿼리를 의미함.
  - 단일값이라고 생각하면 됨.
- 단일 비교 연산자와 함께 주로 활용됨.

예시
```sql
SELECT
  user_name,
  email
FROM users
WHERE age > (SELECT age FROM users WHERE user_id = 10);
```

> `WHERE` 뿐만 아니라 `SELECT` 절, `CASE` 절에서도 주로 사용됨.
{: .prompt-tip }