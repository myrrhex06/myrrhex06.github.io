---
title: "SQL - 외부 조인(OUTER JOIN)이란?"
date: 2025-09-21 12:07:00 +0900
categories: [Language, SQL]
tags: [sql, mysql, select, outerjoin, join]
---

## **외부 조인(OUTER JOIN)이란?**
- 내부 조인(`INNER JOIN`)과는 다르게 `ON` 조건을 만족하지 않는 데이터들도 모두 `JOIN`함.
  - 이때, 데이터가 없는 경우엔 `NULL` 값으로 채움.
- 외부 조인에는 `LEFT OUTER JOIN`, `RIGHT OUTER JOIN`이 있으며, 둘의 차이는 기준이 되는 테이블이 어느 쪽에 위치하는지에 있음.

형식
```sql
SELECT column_name, ...
FROM table_name
[LEFT/RIGHT] OUTER JOIN table_name2
ON table_name_column = table_name2_column;
```

예시
```sql
SELECT
  u.user_id,
  u.name,
  a.article_name,
  a.article_content
FROM users u
LEFT OUTER JOIN articles a
ON u.user_id = a.user_id;
```

> `OUTER` 구문은 생략해서 `LEFT JOIN`, `RIGHT JOIN`으로만 사용하는 것도 가능함.
{: .prompt-tip }

위 예시에서는 `users` 테이블을 기준으로 모든 데이터를 전부 가져오며, `articles` 테이블에 `user_id`가 일치하는 데이터들을 `JOIN`하고, 만약 일치하는 데이터가 없는 경우 `NULL`로 채워서 조회됨.

### **LEFT OUTER JOIN vs RIGHT OUTER JOIN**
두 외부 조인은 서로 거의 유사하지만, 기준이 되는 테이블의 위치가 다름.

- `LEFT OUTER JOIN`의 경우 왼쪽(`FROM` 절)에 위치한 테이블의 모든 데이터를 가져오며, 오른쪽(`JOIN` 절)에 위치한 테이블과 `JOIN`해서 데이터를 가져오는데 만약 `ON` 조건에 일치하는 데이터가 없는 경우엔 `NULL`로 데이터를 가져옴
- `RIGHT OUTER JOIN`의 경우 오른쪽(`JOIN` 절)에 위치한 테이블의 모든 데이터를 가져오며, 왼쪽(`FROM` 절)에 위치한 테이블과 `JOIN`해서 데이터를 가져오는데 만약 `ON` 조건에 일치하는 데이터가 없는 경우엔 `NULL`로 데이터를 가져옴.

> 실무에선 `LEFT JOIN`을 사용하는 빈도가 훨씬 높은데, 그 이유는 기준이 되는 테이블이 먼저 나오고 `JOIN`되는 테이블이 나중에 붙는 형식이 쿼리를 읽기가 훨씬 더 수월하기 때문임.
{: .prompt-tip }