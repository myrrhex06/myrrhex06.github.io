---
title: "SQL - UNION이란?"
date: 2025-10-07 10:50:00 +0900
categories: [Language, SQL]
tags: [sql, mysql, union, select, unionall]
---

## **UNION이란?**
- 서로 다른 `SELECT`문의 결과를 합칠 때 사용함.
- 두 결과를 합친 후에 완전히 중복되는 행은 자동으로 제거해서 고유값만 남게 함.
- 전체 결과를 정렬한 후 인접한 행들을 비교해서 중복을 찾아내는 과정을 거침.
  - 데이터가 많으면 많을수록 오래걸림.

**UNION 사용 규칙**
- `SELECT`문 컬럼 개수가 동일해야함.
- `SELECT`문의 같은 위치에 있는 컬럼들은 서로 호환 가능한 데이터 타입이어야함.
- 최종 결과의 컬럼 이름은 첫번째 `SELECT`문의 컬럼 이름을 따라감.

예시
```sql
SELECT article_id, article_title FROM articles
UNION
SELECT article_id, article_title FROM deleted_articles;
```

> 조회되는 컬럼명은 첫번째 `SELECT` 문을 따라감.
{: .prompt-tip }

## **UNION ALL이란?**
- `UNION`과 유사하지만 중복 데이터를 제거하지 않고 모두 반환함.
- 두 결과를 합친 후에 중복 제거 과정이 필요 없어서 성능이 빠름.
  - `UNION` 보다 데이터 수의 상관 없이 성능이 월등히 빠름.

예시
```sql
SELECT article_id, article_title FROM articles
UNION ALL
SELECT article_id, article_title FROM deleted_articles;
```

## **UNION(or UNION ALL) 결과 정렬**
- `UNION`(또는 `UNION ALL`) 연산 후에 `ORDER BY`절을 통해 정렬 가능함.
- 첫번째 `SELECT`문의 명시된 컬럼명을 통해 정렬해야하며 명시되지 않은 컬럼을 통한 정렬은 불가능함.

구문
```sql
SELECT ...
UNION
SELECT ...
ORDER BY column_name
```

예시
```sql
SELECT article_id, article_title FROM articles
UNION
SELECT article_id, article_title FROM deleted_articles
ORDER BY article_id DESC;
```