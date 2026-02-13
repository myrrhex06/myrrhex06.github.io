---
title: "SQL - SELECT절 서브쿼리"
date: 2025-10-06 18:25:00 +0900
categories: [Language, SQL]
tags: [sql, mysql, subquery, select]
---

## **SELECT절 서브쿼리**
- `SELECT` 절에서 사용되는 서브쿼리는 하나의 컬럼처럼 동작함.
  - 사용되는 서브쿼리는 단일행, 단일열을 반환하는 스칼라 서브쿼리가 사용되는 것임.

예시
```sql
SELECT
  name, 
  views,
  (SELECT AVG(views) FROM articles) as total_views
FROM articles;
```

**쿼리 실행 흐름**

1. 메인쿼리 실행 전 `SELECT` 절에 스칼라 서브쿼리를 먼저 실행함.
2. DB는 이 실행 결과를 기억해둠.
3. 메인쿼리 실행.
4. 테이블의 각 행을 가져올 때 서브쿼리 컬럼에 미리 기억해둔 값을 그대로 추가함.

**스칼라 서브쿼리의 성능 문제**
- `JOIN`으로 표현하기에는 복잡한 로직을 직관적으로 처리해줌
  - 메인쿼리에 행 개수가 많으면 많을수록 성능이 매우매우매우 떨어짐