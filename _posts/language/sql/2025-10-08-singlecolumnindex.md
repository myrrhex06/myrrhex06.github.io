---
title: "SQL - 단일 컬럼 인덱스 활용"
date: 2025-10-08 14:10:00 +0900
categories: [Language, SQL]
tags: [sql, mysql, index, select]
---

## **단일 컬럼 인덱스 활용**
인덱스를 사용하는 상황은 아래와 같음.
- 동등 비교(`=`), `JOIN`
- 범위 검색(`BETWEEN`, `>`, `<`, `LIKE` 등)
- `ORDER BY` 정렬

### **동등 비교**
인덱스 적용 X 예시
```sql
EXPLAIN SELECT * FROM articles WHERE title = 'SQL 인덱스 완벽 정리';
```

결과
![result](/assets/img/equalsindexnotuse.png)

- `type`이 `ALL`로 되어있기 때문에 풀 테이블 스캔으로 처리됨.
- 탐색되는 행은 풀 테이블 스캔이기 때문에 전체 행인 `10`으로 확인됨.

인덱스 적용 예시
```sql
EXPLAIN SELECT * FROM articles WHERE title = 'SQL 인덱스 완벽 정리';
```

결과
![result](/assets/img/equalsindex.png)

- `type`이 `ref`인 것을 확인할 수 있음.
  - 동등 비교(`=`) 또는 `JOIN`에서 인덱스를 사용했다는 의미임.
- `possible_keys`: 현재 쿼리에서 사용 가능한 인덱스 후보를 나타냄.
- 위 결과를 확인했을 때 `idx_articles_title`이라는 인덱스를 탔다고 탐색하게 되는 행이 1개라는 것을 확인할 수 있음.
  - 풀 테이블 스캔에서 탐색되는 행과 비교했을 때 효율이 월등히 좋음.
    - 데이터가 많아질수록 훨씬 더 효율적으로 처리된다는 의미

### **범위 비교**
- `BETWEEN`, `>`, `<` 등과 같은 범위 비교에도 인덱스를 타게할 수 있음.

인덱스 적용 X 예시
```sql
EXPLAIN SELECT * FROM articles WHERE views BETWEEN 1000 AND 1500;
```

결과
![result](/assets/img/rangeindexnotuse.png)

- `type`이 `ALL`로 풀 테이블 스캔으로 처리되는 것을 확인 가능함.

인덱스 적용 예시
```sql
EXPLAIN SELECT * FROM articles WHERE views BETWEEN 1000 AND 1500;
```

결과
![result](/assets/img/rangeindex.png)

- `type`이 `range`인 것을 확인할 수 있음.
  - 인덱스를 통해 특정 범위의 데이터를 스캔하겠다는 의미임.
- 탐색되는 행 수가 줄어든 것을 확인할 수 있음.

쿼리 실행 결과
![result](/assets/img/rangeindexselectresult.png)

- 별도의 `ORDER BY` 구문이 없음에도 인덱스를 통한 범위 스캔이기 때문에 결과가 정렬된 상태로 조회됨.
  - `views`에 적용되어 있는 인덱스가 조건에 맞는 데이터를 순서대로 스캔하면서 조회된 데이터의 기본키(`PK`) 값을 통해 원본 테이블에서 데이터를 가져왔기 때문임.
    - 인덱스에 저장된 데이터들은 모두 정렬되어 있기 때문에 이러한 처리가 가능함.

### **LIKE 범위 검색**
- `LIKE`는 다른 범위 검색들과는 다르게 인덱스를 탈 수 없는 경우의 수가 존재함.
  - `column_name LIKE ‘검색어%’` → 인덱스 사용 가능
  - `column_name LIKE ‘%검색어’` → 인덱스 사용 불가
  - `column_name LIKE ‘%검색어%’` → 인덱스 사용 불가
- 인덱스는 내부적으로 컬럼의 데이터와 기본키 값을 한 쌍으로 저장하게 되는데, `‘%검색어’`, `‘%검색어%’` 이 두 경우는 정렬이 되어있는 데이터일지라도 모든 데이터를 일일이 비교해서 확인해봐야하기 때문에 인덱스를 타는 의미가 없음.

`LIKE` 비교 인덱스 적용 예시
```sql
EXPLAIN SELECT * FROM articles WHERE title LIKE 'SQL%';
```

결과
![result](/assets/img/likeindexsuccess.png)

- 인덱스를 정상적으로 탄 것을 확인할 수 있음.

`LIKE` 비교 인덱스 적용 실패 예시 1
```sql
EXPLAIN SELECT * FROM articles WHERE title LIKE '%SQL%';
```

결과
![result](/assets/img/likeindexfail1.png)

`LIKE` 비교 인덱스 적용 실패 예시 2
```sql
EXPLAIN SELECT * FROM articles WHERE title LIKE '%SQL';
```

결과
![result](/assets/img/likeindexfail2.png)

- 실패 에시들에서는 모두 풀 테이블 스캔으로 처리되는 것을 확인할 수 있음.

### **정렬 처리**
- `ORDER BY`는 데이터가 많으면 많을수록 비용이 많이드는 무거운 작업임.
  - 조건에 맞는 데이터를 찾은 후에 결과를 서로 비교하면서 정렬해야하기 때문임.
- 인덱스를 활용하면 정렬된 인덱스를 순서대로 읽기만 하면 되기 때문에 매우 효율적으로 처리가 가능함.
  - 이는 filesort 과정을 생략하기 때문에 가능한 것임.

> filesort란 메모리나 디스크에 임시 테이블을 만들어 정렬하는 내부 처리 과정을 의미함.
{: .prompt-tip }

인덱스 적용 X 예시
```sql
EXPLAIN SELECT * FROM articles WHERE views BETWEEN 1000 AND 15000 ORDER BY views;
```

결과
![result](/assets/img/filesortcheckresult.png)

- `Extra`에서 `filesort`가 사용되고 있음을 확인할 수 있음.
  - 데이터가 많아질수록 성능이 떨어지게됨.

인덱스 적용 예시
```sql
EXPLAIN SELECT * FROM articles WHERE views BETWEEN 1000 AND 15000 ORDER BY views;
```

결과
![result](/assets/img/filesortunuseresult.png)

- filesort를 타지 않는 것을 확인할 수 있음.
- 이때 `ORDER BY`를 따로 붙이지 않아도 인덱스 상에서 정렬된 상태에서 조회되기 때문에 `ORDER BY`를 생략하고도 동일한 결과를 얻을 수 있음.

`ORDER BY` 생략 쿼리 예시
```sql
SELECT * FROM articles WHERE views BETWEEN 1000 AND 15000;
```

결과
![result](/assets/img/orderbynotuseviewselectresult.png)

내림차순(DESC)를 적용시킬 경우에도 인덱스가 있다면 filesort를 하지 않지만, 인덱스 역방향 스캔이 일어남.

내림차순 적용 예시
```sql
EXPLAIN SELECT * FROM articles WHERE views BETWEEN 1000 AND 15000 ORDER BY views DESC;
```

결과
![result](/assets/img/checkbackwardindexscan.png)

- `Extra`에 `Backward index scan`이 명시된 것을 확인할 수 있음.
  - 인덱스 역방향 스캔을 의미함.

성능상으로는 인덱스를 통한 정렬과 거의 차이가 없지만, 만약 내림차순을 했을 때 역방향 스캔이 일어나는 것을 막기 위해서는 **내림차순 인덱스**를 사용하면 됨.

구문
```sql
CREATE INDEX index_name ON table_name (column_name DESC);
```

예시
```sql
CREATE INDEX idx_articles_views_desc ON articles (views DESC);
```

내림차순 인덱스 적용 실행 계획 확인
```sql
EXPLAIN SELECT * FROM articles WHERE views BETWEEN 1000 AND 15000 ORDER BY views DESC;
```

결과
![result](/assets/img/descindexapplyresult.png)

- 내림차순(DESC) 정렬을 했음에도 `Extra`에 `Backward index scan`이 사라진 것을 확인할 수 있음.
  - 인덱스에 데이터를 저장할 때 내림차순 정렬을 하기 때문임.
  - 이 경우엔 오름차순(ASC) 정렬을 할 때 `Backward index scan`을 하게됨.