---
title: "SQL - NULL 함수"
date: 2025-09-19 20:10:00 +0900
categories: [Language, SQL]
tags: [sql, mysql, ifnull, select, coalesce]
---

## **IFNULL**
형식
```sql
SELECT IFNULL(column_name, default_value)
FROM table_name
```

- `IFNULL` 함수는 넘어온 컬럼이 `NULL`일 경우 기본값을 가져오는 함수임.

예시
```sql
SELECT IFNULL(nickname, '무명사용자')
FROM user_info;
```

## **COALESCE**
형식
```sql
SELECT COALESCE(column_name1, ..., default_value)
FROM table_name;
```

- `COALESCE` 함수는 넘어온 인자값들 중 처음으로 `NULL`이 아닌 값을 반환함

```sql
SELECT COALESCE(article_description, article_content, '내용 없음')
FROM article_info;
```

> `IFNULL` 함수는 두 개의 인자만 받을 수 있지만, `COALESCE`는 제한이 없기 때문에 더 유연하게 사용이 가능함.
{: .prompt-tip }