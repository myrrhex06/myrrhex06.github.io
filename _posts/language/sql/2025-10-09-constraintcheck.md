---
title: "CHECK 제약 조건이란?"
date: 2025-10-09 15:45:00 +0900
categories: [Language, SQL]
tags: [sql, mysql, constraint, check]
---

> [예전글](https://myrrhex06.github.io/posts/check/)에서 간략하게 다뤘었던 내용을 보충하는 글임.

## **CHECK 제약 조건이란?**
- 컬럼에 들어갈 수 있는 값의 범위나 조건을 직접 지정할 때 사용함.
- 컬럼에 `INSERT`, `UPDATE`가 일어날 때 지정된 조건식이 참인지 검사함.
  - 거짓이면 입력을 거부하고 에러를 발생시킴.

사용 예시
```sql
CREATE TABLE articles (
    article_id BIGINT AUTO_INCREMENT PRIMARY KEY,
    title VARCHAR(255) NOT NULL,
    category VARCHAR(100),
    views BIGINT DEFAULT 0 CHECK (views >= 0)
);
```

**참고**
아래 형태로도 제약 조건 추가가 가능함.
```sql
CONSTRAINT constraint_name CHECK (condition_expression);
```

예시
```sql
CREATE TABLE articles (
    article_id BIGINT AUTO_INCREMENT PRIMARY KEY,
    title VARCHAR(255) NOT NULL,
    category VARCHAR(100),
    views BIGINT DEFAULT 0,
    CONSTRAINT chk_views CHECK (views >= 0)
);
```