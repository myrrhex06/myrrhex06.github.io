---
title: "PostgreSQL - Lock 조회 및 세션 종료 쿼리"
date: 2026-09-30 22:20:00 +0900
categories: [Language, SQL]
tags: [sql, postgresql, transaction, session, lock]
---

쿼리
```sql
SELECT * 
FROM pg_stat_activity
WHERE state != 'idle';


SELECT 
    pid,
    usename AS username,
    datname AS dbname,
    state,
    query,
    query_start,
    xact_start,
    wait_event_type,
    wait_event
FROM pg_stat_activity
WHERE state != 'idle';

SELECT
    a.pid,
    a.usename,
    a.client_addr,
    a.query,
    a.query_start,
    l.locktype,
    l.mode,
    l.granted
FROM pg_stat_activity a
JOIN pg_locks l
    ON a.pid = l.pid
WHERE a.state != 'idle'
ORDER BY a.query_start;

SELECT pg_terminate_backend(<PID>);
```