---
title: "SQL - 안전 업데이트 모드란?"
date: 2025-09-09 21:33:00 +0900
categories: [Language, SQL]
tags: [sql, mysql, safe, update, delete, dml, where, mode]
---

## **안전 업데이트 모드란?**
일반적으로 `UPDATE` 또는 `DELETE`를 사용할 땐 아래와 같이 `WHERE` 절을 통해 수정 또는 삭제할 데이터를 지정한다.

```sql
-- update
UPDATE test 
SET email = 'test@gmail.com'
WHERE test_id = 1;

-- delete
DELETE FROM test
WHERE test_id = 1;
```

만약 `WHERE` 절을 붙이지 않고 `UPDATE` 또는 `DELETE`를 실행할 경우 전체 데이터에 실행 결과가 적용되는 대형 사고가 발생하기 때문에 이를 막기 위한 것이 안전 업데이트 모드이다.

안전 업데이트 모드는 기본키(PK) 또는 인덱스가 붙어 있는 컬럼을 `WHERE` 절에 사용하면 잘 실행되지만, 그 외 컬럼을 `WHERE`에 사용할 경우 에러를 발생시킨다.

> `WHERE`에 PK(또는 인덱스가 붙은 컬럼)를 사용하지 않더라도 `LIMIT`을 사용하면 안전 업데이트 모드가 에러를 발생시키지 않고 잘 실행시키지만, 권장되는 방식이 아니다.
{: .prompt-tip }

**안전 업데이트 모드 활성화 확인**
```sql
SELECT @@SQL_SAFE_UPDATES;
```

- `1`: 활성화
- `0`: 비활성화

> MySQL Workbench를 사용할 경우 활성화 되어 있지만, 그 외 DB 툴은 대부분 비활성화가 기본값이다.

**안전 업데이트 모드 활성화/비활성화 설정**
```sql
SET SQL_SAFE_UPDATES = 1; -- 활성화

SET SQL_SAFE_UPDATES = 0; -- 비활성화
```