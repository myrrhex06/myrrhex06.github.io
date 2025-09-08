---
title: "alter 쿼리 사용 방법"
date: 2025-09-08 21:24:00 +0900
categories: [Language, SQL]
tags: [sql, mysql, alter, table, ddl]
---

## **Alter 쿼리**
- 이미 만들어진 테이블의 구조를 변경할 때 사용

### **컬럼 추가**
구조
```sql
ALTER TABLE [테이블명] ADD COLUMN [컬럼명] [데이터타입] [제약조건];
```

예시
```sql
ALTER TABLE test_user ADD COLUMN email VARCHAR(150) NOT NULL;
```
### **컬럼 수정**
구조
```sql
ALTER TABLE [테이블명] MODIFY COLUMN [컬럼명] [데이터타입] [제약조건];
```

예시
```sql
ALTER TABLE test_user MODIFY COLUMN email VARCHAR(100) NOT NULL;
```

### **컬럼 삭제**
구조
```sql
ALTER TABLE [테이블명] DROP COLUMN [컬럼명];
```

예시
```sql
ALTER TABLE test_user DROP COLUMN email;
```

### **주의 사항**
- 대량의 데이터가 저장되어 있는 테이블의 구조를 변경할 경우 많은 자원과 시간이 소모됨.
- `ALTER` 처리가 완료될 때 까지 테이블이 잠금 상태가 되기 때문에 운영 서버에서 사용되고 있는 DB는 되도록 건드리지 않는게 좋음.
  - 만약 건드려야한다면 사용자가 적은 새벽 시간 또는 점검 시간에 변환하는게 좋음.