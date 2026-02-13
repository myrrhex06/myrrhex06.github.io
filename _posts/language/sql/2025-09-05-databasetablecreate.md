---
title: "SQL - 데이터베이스와 테이블 생성, 조회, 제거 관련 쿼리"
date: 2025-09-05 21:40:00 +0900
categories: [Language, SQL]
tags: [sql, mysql, ddl]
---

## **데이터베이스**

**구성된 데이터베이스 목록 확인**
```sql
SHOW DATABASES;
```

결과

![image](/assets/img/showdatabasesresult.png)

**데이터베이스 생성**
```sql
CREATE DATABASE 데이터베이스명;
```

예시
```sql
CREATE DATABASE test;
```

**사용할 데이터베이스 지정**
```sql
USE 데이터베이스명;
```

- `USE 데이터베이스명` 쿼리를 실행한 후 작성하는 모든 쿼리는 해당 데이터베이스 내에서 실행된다.

**데이터베이스 제거**
```sql
DROP DATABASE 데이터베이스명;
```

## **테이블**

**테이블 생성**
```sql
CREATE TABLE 테이블명(
  컬럼명 타입 제약조건,
  컬럼명 타입 제약조건,
  ...
)
```

예시
```sql
CREATE TABLE user_info(
	user_seq BIGINT AUTO_INCREMENT,
    email VARCHAR(150) NOT NULL,
    password VARCHAR(200) NOT NULL,
    nickname VARCHAR(100),
    created_at DATETIME,
    modified_at DATETIME,
    PRIMARY KEY(user_seq)
);
```

- 테이블 생성 시 기본키(PRIMARY KEY)는 반드시 지정해줘야한다.

**구성된 테이블 확인**

```sql
SHOW TABLES;
```

결과

![image](/assets/img/showtablesresult.png)

**테이블 구조 확인**

```sql
DESC 테이블명;
-- 또는 DESCRIBE 테이블명;
```

예시
```sql
DESCRIBE user_info;
```

결과

![image](/assets/img/desctableresult.png)

**테이블 제거**

```sql
DROP TABLE 테이블명;
```