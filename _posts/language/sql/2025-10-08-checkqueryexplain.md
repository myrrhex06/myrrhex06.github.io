---
title: "SQL - 쿼리 실행 계획이란?"
date: 2025-10-08 13:25:00 +0900
categories: [Language, SQL]
tags: [sql, mysql, index, select, explain]
---

## **쿼리 실행 계획이란?**
- 실행하려는 쿼리가 어떤 방식으로 실행되는지, 인덱스를 타는지 안타는지를 확인하는 것.
  - 실행하려는 쿼리 앞에 `EXPLAIN` 키워드를 붙여 확인 가능함.

구문
```sql
EXPLAIN query;
```

예시
```sql
EXPLAIN SELECT * FROM articles;
```

결과
![result](/assets/img/fulltablescanexplainresult.png)

- `type`: 데이터베이스가 테이블에 어떻게 접근할지를 나타냄.
  - `ALL`: 풀 테이블 스캔을 의미함.
  - `ref`: `=` 조건 또는 `JOIN`에서 인덱스를 사용하겠다는 것을 의미함.
  - `range`: 범위 검색(`BETWEEN`, `>`, `<` 등)에서 인덱스를 사용하겠다는 것을 의미함.
- `key`: 쿼리를 실행할 때 사용한 인덱스를 보여줌.
  - 사용한 인덱스가 없을 경우 `NULL이` 표시됨.
- `rows`: 쿼리 실행 시 탐색하게 되는 행(row) 수를 의미함.
  - 풀 테이블 스캔이 실행되기 때문에 전체 테이블 데이터 수가 나타남.
  - 실제 실행 값이 아닌 예측이기 때문에 확정은 아님.
- `filtered`: 테이블에서 읽어온 행들 중에서 `WHERE` 조건으로 필터링되고 난 후 최종적으로 남을 것으로 예측되는 행의 비율
  - 확정된 값이 아닌 대략적인 예측치기 때문에 정확하지 않음.