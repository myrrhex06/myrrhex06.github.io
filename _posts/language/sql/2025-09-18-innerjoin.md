---
title: "SQL - 내부 조인(INNER JOIN)이란?"
date: 2025-09-18 20:25:00 +0900
categories: [Language, SQL]
tags: [sql, mysql, select, innerjoin, join]
---

## **내부 조인이란?**
- 정규화가 적용되어 분리된 테이블들을 하나로 합쳐줌
  - 내부 조인은 조건에 일치하는 데이터만 가져옴 

형식
```sql
SELECT column_name, ...
FROM table_name
[INNER] JOIN table_name2
ON table_name.column = table_name2.column;
```

- `JOIN table_name2`: 연결되는 테이블 지정
- `ON ...`: JOIN 조건 지정

예시
```sql
SELECT
  user_info.name,
  user_info.email,
  article_info.name,
  article_info.content
FROM user_info
JOIN article_info
ON user_info.user_id = article_info.user_id;
```

- `user_info` 테이블의 `user_id`와 `article_info` 테이블의 `user_id`가 일치하는 데이터들만 `JOIN`해서 조회함

> `INNER JOIN`은 `INNER`를 제외하고 `JOIN`만 작성하더라도 `INNER JOIN`으로 인식해서 처리됨.
{: .prompt-tip }

보통 컬럼명에는 `AS` 키워드를 사용하여 별칭을 지정하고, 테이블에는 `AS` 키워드를 생략하고 별칭을 지정하여 사용함

예시
```sql
SELECT
  u.name AS user_name,
  u.email AS email,
  a.name AS article_name,
  a.content AS article_content
FROM user_info u
JOIN article_info a
ON u.user_id = a.user_id;
```