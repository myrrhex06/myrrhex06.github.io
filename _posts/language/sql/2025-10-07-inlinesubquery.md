---
title: "SQL - 테이블 서브쿼리란?"
date: 2025-10-07 09:51:00 +0900
categories: [Language, SQL]
tags: [sql, mysql, subquery, select]
---

## **테이블 서브쿼리란?**
- 서브쿼리 결과가 하나의 독립된 가상 테이블처럼 사용됨.
  - 서브쿼리 결과를 먼저 집합으로 만들어두고 메인 쿼리가 실행됨.
- 주로 복잡한 데이터를 단계적으로 가공해야할 때 매우 유용함.
- 서브쿼리 결과를 `JOIN`, `WHERE`, `FROM`절에서 활용 가능함.

> 인라인 뷰(inline view)라고도 말함
{: .prompt-tip }

예시
```sql
SELECT 
  a.article_id,
  a.views,
  a.title
FROM articles a
JOIN (
  SELECT category, MAX(views) AS total_views
  FROM articles
  GROUP BY category
) AS al
ON a.category = al.category AND a.views = al.total_views
```