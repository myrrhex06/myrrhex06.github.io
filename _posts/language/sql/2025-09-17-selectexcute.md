---
title: "SELECT 실행 순서"
date: 2025-09-17 21:10:00 +0900
categories: [Language, SQL]
tags: [sql, mysql, select]
---

## **SELECT 쿼리 실행 순서**
1. `FROM`: 조회하려는 테이블에서 데이터 가져옴
2. `WHERE`: 테이블에서 가져온 데이터 필터링
3. `GROUP BY`: 데이터 그룹핑
4. `HAVING`: 그룹핑된 데이터 필터링
5. `SELECT`: 조회할 컬럼 설정
6. `ORDER BY`: 정렬


```sql
FROM -> WHERE -> GROUP BY -> HAVING -> SELECT -> ORDER BY
```

