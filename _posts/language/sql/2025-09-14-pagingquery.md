---
title: "페이징 쿼리 구현 방법"
date: 2025-09-14 07:00:00 +0900
categories: [Language, SQL]
tags: [sql, mysql, limit, paging, offset]
---

## **페이징 쿼리 구현**
형식
```sql
SELECT 
  column1, 
  column2, 
  ....
FROM table_name
ORDER BY column_name [ASC | DESC]
LIMIT [number] OFFSET [number]
```

- `LIMIT`: 조회 결과 데이터 개수 제한
  - 특정 구간의 데이터만 잘라 보고싶을 경우 사용
- `OFFSET`: 앞에서부터 몇개의 데이터를 건너뛸건지 지정

**`OFFSET` 구하는 공식**
```
(페이지 수 - 1) * 자를 데이터 수
```

예시
```sql
SELECT *
FROM user_info
ORDER BY name DESC
LIMIT 5 OFFSET 0;
```

`LIMIT`에 `OFFSET`을 같이 넣는것도 가능


예시
```sql
SELECT *
FROM user_info
ORDER BY name DESC
LIMIT 0, 5;
```