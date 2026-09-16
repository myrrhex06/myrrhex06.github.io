---
title: "PostgreSQL - 테이블, 컬럼 정보 추출 쿼리"
date: 2026-09-16 20:00:00 +0900
categories: [Language, SQL]
tags: [sql, postgresql, database, table, column, catalog]
---

## **테이블 정보 추출**

사용 쿼리
```sql
SELECT
    -- 물리DB명: 이 정보는 PostgreSQL 시스템 카탈로그에 명확히 정의되어 있지 않으므로, 데이터베이스 이름을 사용하거나 고정된 값을 사용
    current_database() AS "물리DB명",

    -- 테이블소유자: pg_class와 pg_get_userbyid 함수를 사용하여 소유자명을 동적으로 가져옴.
    pg_catalog.pg_get_userbyid(c.relowner) AS "테이블소유자",

    -- 테이블 영문명: pg_class에서 테이블 이름을 가져옴.
    UPPER(c.relname) AS "테이블 영문명",

    -- 연관 엔터티명: 이 정보는 DB 메타데이터에 없으므로, 테이블 설명을 기반으로 유추하거나 임의의 값을 사용해야함. 여기서는 테이블 설명을 사용함.
    COALESCE(obj_description(c.oid, 'pg_class'), c.relname) AS "연관 엔터티명",

    -- 테이블 유형: PostgreSQL의 relkind를 기반으로 '일반' 테이블(r)만 필터링하고 고정 값을 지정함.
    CASE c.relkind
        WHEN 'r' THEN '일반' -- 일반 테이블 (Relation)
        WHEN 'v' THEN '뷰'
        WHEN 'm' THEN '물리화된 뷰'
        WHEN 'f' THEN '외부 테이블'
        WHEN 'p' THEN '파티션 테이블'
        ELSE '기타'
        END AS "테이블 유형",

    -- 관련엔터티명: 연관 엔터티명과 동일하게 처리함.
    COALESCE(obj_description(c.oid, 'pg_class'), c.relname) AS "관련엔터티명",

    -- 테이블설명: obj_description 함수를 사용하여 코멘트(주석)를 가져옴.
    COALESCE(obj_description(c.oid, 'pg_class'), '') AS "테이블설명",

    '수시' AS "발생주기"

FROM
    pg_catalog.pg_class c
        LEFT JOIN
    pg_catalog.pg_namespace n ON n.oid = c.relnamespace
WHERE
    c.relkind IN ('r', 'p') -- 'r': 일반 테이블, 'p': 파티션 테이블
  AND n.nspname = '스키마명'
ORDER BY
    c.relname;
```

## **컬럼 정보 추출**

사용 쿼리
```sql
select table_name
     ,(select obj_description(t.tablename::regclass, 'pg_class') AS table_comment from pg_tables t where a.table_name = upper(t.tablename) and a.table_schema = t.schemaname)
     , column_name
     , comment
     , case when length is null then type
            when length is not null then type||'('||length||')'
         end datatype
     , type
     , length
     , column_default
     , is_nullable
     , pk
  from (
SELECT
    upper(info.TABLE_NAME) as TABLE_NAME,
    comm.column_comment as comment,
    upper(info.COLUMN_NAME) as COLUMN_NAME,
    case when upper(info.udt_name) ='BPCHAR' then 'CHAR' else upper(info.udt_name) END as type,
    case when upper(info.udt_name) = 'INT8' then NULL
         when info.character_maximum_length is null then info.numeric_precision::VARCHAR||case when numeric_scale =0 then ''
                                                                                               when numeric_scale is not null then ','||numeric_scale end
         else info.character_maximum_length::VARCHAR end as length,
    info.column_default,
    case when info.is_nullable ='YES' then 'N' when info.is_nullable='NO' then 'Y' end is_nullable,
    comm.column_comment as column_comment,
    case when pri_key.column_name is null then '' else 'Y' end as PK,
    info.table_schema
FROM
    information_schema. COLUMNS info
LEFT JOIN (
    SELECT
        PS.schemaname as SCHEMA_NAME,
        PS.RELNAME AS TABLE_NAME,
        PA.ATTNAME AS COLUMN_NAME,
        PD.DESCRIPTION AS COLUMN_COMMENT
    FROM
        PG_STAT_ALL_TABLES PS,
        PG_DESCRIPTION PD,
        PG_ATTRIBUTE PA
    WHERE
        PS.RELID = PD.OBJOID
    AND PD.OBJSUBID <> 0
    AND PD.OBJOID = PA.ATTRELID
    AND PD.OBJSUBID = PA.ATTNUM
    ORDER BY
        PS.RELNAME,
        PD.OBJSUBID
) comm ON comm.SCHEMA_NAME = info.table_schema
AND comm. TABLE_NAME = info. TABLE_NAME
AND comm. COLUMN_NAME = info. COLUMN_NAME
LEFT JOIN (
    SELECT
        CC.*
    FROM
        INFORMATION_SCHEMA.TABLE_CONSTRAINTS TC,
        INFORMATION_SCHEMA.CONSTRAINT_COLUMN_USAGE CC
    WHERE
        TC.CONSTRAINT_TYPE = 'PRIMARY KEY'
   AND TC.TABLE_CATALOG   = CC.TABLE_CATALOG
   AND TC.TABLE_SCHEMA    = CC.TABLE_SCHEMA
   AND TC.TABLE_NAME      = CC.TABLE_NAME
   AND TC.CONSTRAINT_NAME = CC.CONSTRAINT_NAME
) pri_key ON pri_key.table_schema = info.table_schema
AND pri_key. table_name = info.TABLE_NAME
AND pri_key. column_name = info. COLUMN_NAME
WHERE
    info.table_schema = '스키마명'
ORDER BY
    info. TABLE_NAME,
    info.ordinal_position
    ) a;
```