---
title: "SQL - 자동 날짜 입력/갱신 설정 쿼리"
date: 2025-09-07 20:25:00 +0900
categories: [Language, SQL]
tags: [sql, mysql, pk, fk, datetime, currenttimestamp, default]
---

## **자동 날짜 입력/갱신 설정**
예시
```sql
CREATE TABLE test(
  test_id INT AUTO_INCREMENT,
  created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
  updated_at DATETIME DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY(test_id)
);
```

결과 확인
```sql
DESC test;
```

결과
![image](/assets/img/current_timestamp_result.png)

```sql
  ...
  created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
  updated_at DATETIME DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  ...
```
- `DEFAULT CURRENT_TIMESTAMP`: 데이터 삽입 시 해당 열에 데이터를 저장하지 않을 경우 자동으로 현재 날짜 입력
- `ON UPDATE CURRENT_TIMESTAMP`: 데이터 수정 시 자동으로 현재 날짜로 갱신