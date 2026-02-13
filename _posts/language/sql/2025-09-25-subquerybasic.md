---
title: "SQL - 서브쿼리의 기본 개념"
date: 2025-09-25 21:46:00 +0900
categories: [Language, SQL]
tags: [sql, mysql, subquery, select]
---

## **서브쿼리란?**
- 하나의 SQL문 안에 포함된 또 다른 `SELECT` 쿼리를 의미함.
- 서브쿼리는 메인쿼리가 실행되기 전에 먼저 실행됨.
  - 이때, DB는 서브쿼리의 결과를 메인쿼리에 전달하여 메인쿼리가 그 결과를 사용해서 작업을 수행하게 됨.

예시
```sql
SELECT 
  article_name,
  article_content
FROM articles
WHERE views > (SELECT AVG(views) FROM articles);
```

> 서브쿼리는 반환하는 행(row)과 컬럼(column)의 수에 따라 종류가 나뉘며 사용되는 위치와 연산자에 따라 역할이 결정됨.
{: .prompt-tip }