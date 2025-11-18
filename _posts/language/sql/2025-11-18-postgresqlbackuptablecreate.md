---
title: "PostgreSQL 백업 테이블 생성하기"
date: 2025-11-18 18:03:00 +0900
categories: [Language, SQL]
tags: [sql, postgresql, create, ddl, select]
---

백업 테이블 생성 쿼리
```sql
CREATE TABLE backup_table_name
AS SELECT * FROM original_table_name;
```