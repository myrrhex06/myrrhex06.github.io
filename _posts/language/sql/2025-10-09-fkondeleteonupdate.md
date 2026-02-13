---
title: "SQL - 외래키 제약 조건 ON DELETE, ON UPDATE 옵션이란?"
date: 2025-10-09 15:40:00 +0900
categories: [Language, SQL]
tags: [sql, mysql, constraint, fk, cascade, restrict, setnull]
---

## **ON DELETE / ON UPDATE**
- DB는 외래키 제약 조건이 걸려 있는 상위 데이터의 삭제나 수정을 막는 것(`RESTRICT`)이 기본값임.
  - 가장 안전한 정책임.
- 상위 테이블의 데이터가 삭제되면 그 데이터를 참조하는 하위 테이블의 데이터도 같이 삭제되도록 옵션을 줄 수 있음.

**옵션 종류**

| 옵션                | 설명                                                                                                                   |
| ------------------- | ---------------------------------------------------------------------------------------------------------------------- |
| `RESTRICT` (기본값) | 하위 테이블에 참조 중인 행이 있으면 상위 테이블의 행을 삭제/수정할 수 없음.                                            |
| `CASCADE`           | 상위 테이블의 행이 삭제/수정되면, 그 행을 참조하는 하위 테이블의 행도 함께 삭제/수정됨.                                |
| `SET NULL`          | 상위 테이블의 행이 삭제/수정되면, 하위 테이블의 외래 키 컬럼 값을 `NULL`로 변경. 단, 하위 컬럼이 `NULL` 허용이어야 함. |


예시
```sql
CREATE TABLE articles (
    article_id BIGINT AUTO_INCREMENT,
    user_id BIGINT NOT NULL,
    title VARCHAR(100) NOT NULL,
    content TEXT NOT NULL,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    PRIMARY KEY (article_id),
    CONSTRAINT fk_articles_users 
        FOREIGN KEY (user_id)
        REFERENCES users(user_id) 
        ON DELETE CASCADE
        ON UPDATE CASCADE
);
```

`CASCADE` 옵션은 상위 데이터가 삭제/수정될 경우 연관된 하위 데이터를 모두 같이 삭제/수정 처리하기 때문에 사용 시 주의가 필요함.

> 대부분의 경우 기본키(`PK`) 값이 변하는 경우는 없기 때문에 `ON UPDATE`는 잘 사용되지 않음.
{: .prompt-tip }