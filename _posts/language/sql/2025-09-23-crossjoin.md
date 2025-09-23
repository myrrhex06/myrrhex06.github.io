---
title: "크로스 조인(CROSS JOIN)이란?"
date: 2025-09-23 20:20:00 +0900
categories: [Language, SQL]
tags: [sql, mysql, select, crossjoin, join]
---

## **크로스 조인(CROSS JOIN)이란?**
- ON절 없이 한쪽 테이블의 모든 행을 다른쪽 테이블의 모든 행과 하나씩 `JOIN`하는 것을 의미함.
  - A 테이블의 전체 데이터 m개, B 테이블의 전체 테이터 n개를 `CROSS JOIN`하면 m * n개의 결과가 나오게됨.

> 크로스 조인 결과를 카테시안 곱(또는 데카르트 곱)이라고 함.
{: .prompt-tip }

형식
```sql
SELECT 
  column_name,
  ...
FROM table_name_a
CROSS JOIN table_name_b
```

예시
```sql
SELECT
  b.brand,
  c.name
FROM brands b
CROSS JOIN cloths c
```

> 크로스 조인은 모든 행을 하나씩 연결하기 때문에 특정 상황에서 유용하게 사용되지만, `JOIN` 되는 경우의 수가 많아질수록 DB 성능의 영향이 가기 때문에 주의해서 사용해야함.
{: .prompt-tip }