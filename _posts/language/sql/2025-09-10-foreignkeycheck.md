---
title: "SQL - 외래키 제약 조건 옵션 해제 방법"
date: 2025-09-10 21:33:00 +0900
categories: [Language, SQL]
tags: [sql, mysql, fk, check]
---

## **외래키(FOREIGN KEY) 제약 조건 옵션**
기본적으로 참조되고 있는 상위 테이블에 데이터는 삭제가 불가능하지만, 외래키 제약 조건 옵션을 비활성화 시키면 삭제가 가능해진다.

**외래키 제약 조건 옵션 조회**
```sql
SELECT @@FOREIGN_KEY_CHECKS
```

- 1: 활성화
- 0: 비활성화

**외래키 제약 조건 옵션 비활성화**
```sql
SET FOREIGN_KEY_CHECKS = 0; -- 비활성화
SET FOREIGN_KEY_CHECKS = 1; -- 활성화
```
