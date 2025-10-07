---
title: "뷰(View)란?"
date: 2025-10-07 16:35:00 +0900
categories: [Language, SQL]
tags: [sql, mysql, view, select]
---

## **뷰(View)란?**
- 가상의 테이블을 의미함.
- 실제 데이터를 저장하지 않고, 미리 정의된 `SELECT` 쿼리를 저장해두는 개념임.
- 기본적으로 조회용 테이블로 사용됨.

구문
```sql
CREATE VIEW view_name AS SELECT ...
```

예시
```sql
CREATE VIEW v_notifications_send_status AS
SELECT 
  COUNT(*) AS total_send,
  SUM(CASE WHEN status = 'COMPLETED' THEN 1 ELSE 0 END) AS completed_count,
  SUM(CASE WHEN status = 'PROGRESS' THEN 1 ELSE 0 END) AS progress_count,
  SUM(CASE WHEN status = 'PENDING' THEN 1 ELSE 0 END) AS pending_count
FROM notifications;
```

> 일반적으로 뷰이름에는 `v_` 또는 `view_` 같은 접두사를 붙여서 구분함.
{: .prompt-tip }

**뷰의 동작 원리**

1. 사용자가 `SELECT` 쿼리를 실행함.
2. DB는 실제 테이블을 찾는 대신 뷰(view)에 정의된 원래의 긴 `SELECT` 쿼리문을 찾아냄.
3. DB는 저장된 `SELECT` 쿼리문을 실행함.
4. 실행 결과가 사용자에게 반환됨.

> 뷰는 데이터를 저장하지 않고 뷰를 조회할 때마다 항상 최신 상태의 원본 테이블을 기준으로 쿼리가 실행되기 때문에 뷰의 데이터는 항상 최신 상태를 유지함.
{: .prompt-tip }

### **뷰 수정**
- `ALTER`를 사용해 기존 뷰를 수정할 수 있음.

구문
```sql
ALTER VIEW view_name AS SELECT ...
```

예시
```sql
ALTER VIEW v_notifications_send_status AS
SELECT
  SUM(CASE WHEN status = 'COMPLETED' THEN 1 ELSE 0 END) AS completed_count,
  SUM(CASE WHEN status = 'PROGRESS' THEN 1 ELSE 0 END) AS progress_count,
  SUM(CASE WHEN status = 'PENDING' THEN 1 ELSE 0 END) AS pending_count
FROM notifications;
```

### **뷰 제거**
- `DROP`을 사용해 뷰를 제거할 수 있음.

구문
```sql
DROP VIEW view_name;
```

예시
```sql
DROP VIEW v_notifications_send_status;
```

### **뷰의 장/단점**
**장점**
- 편리성과 재사용성: 복잡한 `SELECT` 쿼리를 뷰 뒤에 감춰둘 수 있음.
- 보안성: 데이터베이스에 대한 섬세한 권한 제어가 가능함.
- 추상화: 뷰는 추상화 되어있기 때문에 조회하는 입장에서는 뷰를 구성하는 `SELECT` 쿼리가 변경되더라도 상관 없이 동일한 조회 쿼리를 유지할 수 있음.

**단점**
- 성능 문제: 뷰는 실행 시마다 원본 테이블을 조회하기 때문에, 복잡한 뷰를 중첩하거나 대용량 데이터에 사용하면 실행 속도가 크게 느려질 수 있음.
- 관리 복잡성: 여러 뷰가 서로를 참조할 경우, 구조 변경 시 수정 범위가 넓어질 수 있음.