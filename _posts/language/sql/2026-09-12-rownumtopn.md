---
title: "Oracle - ROWNUM으로 상위 데이터 추출하기"
date: 2026-09-12 15:01:00 +0900
categories: [Language, SQL]
tags: [sql, oracle, database, rownum, topn]
---

Oracle에서는 `ROWNUM`이라는 값을 이용해서 상위 N개의 데이터를 추출할 수 있음.

`ROWNUM`은 Oracle이 조회 결과에 있는 각 행에 부여하는 순번같은 개념임.

> `ROWNUM` 값은 1부터 시작함.
{: .prompt-tip }

`ROWNUM`은 `ORDER BY`보다 더 먼저 계산되기 때문에, 아래와 같이 쿼리를 작성할 경우 정상적으로 동작하지 않음.

```sql
SELECT
	*
FROM EMP_SALES
WHERE ROWNUM <= 3
ORDER BY SALES_AMT DESC;
```

정렬이 필요하다면 아래와 같이 서브쿼리를 통해서 먼저 `ORDER BY`로 정렬해줘야함.

```sql
SELECT
	*
FROM 
	(
		SELECT 
			*
		FROM EMP_SALES
		ORDER BY SALES_AMT DESC
	)
WHERE ROWNUM <= 3;
```