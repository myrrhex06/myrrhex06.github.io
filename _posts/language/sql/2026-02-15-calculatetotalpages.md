---
title: "SQL - 전체 페이지 수 구하기"
date: 2026-02-15 14:01:00 +0900
categories: [Language, SQL]
tags: [sql, paging, size, page, mysql]
---

구문
```sql
SELECT
	(COUNT(*) + size - 1) / size AS total_pages
FROM table_name
```