---
title: "다중 행 서브쿼리란?"
date: 2025-09-28 09:35:00 +0900
categories: [Language, SQL]
tags: [sql, mysql, subquery, select]
---

## **다중 행 서브쿼리란?**
- 서브쿼리의 결과가 여러 행으로 반환될 때 사용함.
  - `IN`, `ANY`, `ALL` 연산자와 주로 사용함.

예시
```sql
SELECT * 
FROM users 
WHERE id IN (SELECT user_id FROM articles WHERE category = '건강');
```

### **ANY, ALL 연산자**
- 비교 연산자와 함께 사용되며, 서브쿼리가 반환하는 여러 값들과 비교하는 역할을 수행함.
  - `ANY`: 서브쿼리로 반환되는 값들과 비교했을 때 하나만 조건에 만족해도 `true`
  - `ALL`: 서브쿼리로 반환되는 값들과 비교했을 때 모두 조건을 만족해야 `true`

예시
```sql
SELECT *
FROM articles
WHERE likes > ANY (SELECT likes FROM articles WHERE category = '운동');
```