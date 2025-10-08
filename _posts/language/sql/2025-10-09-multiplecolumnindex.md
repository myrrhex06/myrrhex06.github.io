---
title: "복합 컬럼 인덱스란?"
date: 2025-10-09 08:07:00 +0900
categories: [Language, SQL]
tags: [sql, mysql, index, select, in, where]
---

## **복합 컬럼 인덱스란?**
- 두 개 이상의 컬럼을 묶어서 하나의 인덱스를 만드는 것을 말함.
  - 다중 조건 쿼리의 성능을 최적화하기 위해 사용됨.
- `WHERE`절에서 사용되는 컬럼의 순서가 성능을 좌우함.

복합 인덱스 생성 구문
```sql
CREATE INDEX index_name ON table_name (column1, column2, ...);
```

예시
```sql
CREATE INDEX idx_articles_title_views ON articles (title, views);
```

### **컬럼 순서가 중요한 이유**
- 예를 들어 `articles` 테이블에 `title`, `views` 순서로 복합 인덱스를 만들었다고 가정함.
    - 인덱스는 내부적으로 `title`을 기준으로 먼저 정렬함.
    - 같은 `title` 내에서 `views`를 기준으로 다시 정렬함.
    - 이때 (`title`), (`title`, `views`) `WHERE` 절은 인덱스를 탈 수 있지만, (`views`)만을 사용한 `WHERE`절은 인덱스를 타지 못함.
        - 인덱스를 타는 것에 전제 조건은 정렬이 되어 있어야하는데 `views`는 `title`이 먼저 정렬된 후 같은 `title`을 기준으로 `views`를 정렬한 것이기 때문에 `views`만을 사용해서 `WHERE` 절을 사용하면 인덱스를 제대로 탈 수 없음.
- 첫 번째 컬럼을 기준으로 정렬된 상태에서만 제 역할을 할 수 있음.


### 복합 인덱스 사용 예시
예시 1
```sql
-- 인덱스 생성 구문 --
CREATE INDEX idx_articles_title_views ON articles (title, views);

EXPLAIN SELECT * FROM articles WHERE title = 'SQL 인덱스 완벽 정리';
```

- 인덱스의 첫번째 컬럼으로 사용된 `title`만을 `WHERE` 절에서 사용함.

결과
![result](/assets/img/multiplecolumnindextitlecheck.png)

- 인덱스를 제대로 탄 것을 확인할 수 있음.

예시 2
```sql
-- 인덱스 생성 구문 --
CREATE INDEX idx_articles_title_views ON articles (title, views);

EXPLAIN SELECT * FROM articles WHERE title = 'SQL 인덱스 완벽 정리' AND views >= 1000;
```

결과
![result](/assets/img/multiplecolumnindextitleviewscheck.png)

- 마찬가지로 인덱스를 제대로 탄 것을 확인할 수 있음.
- 이때, `WHERE`절의 사용되는 두 컬럼의 위치를 바꾸더라도 동일하게 동작함.

```sql
-- 인덱스 생성 구문 --
CREATE INDEX idx_articles_title_views ON articles (title, views);

EXPLAIN SELECT * FROM articles WHERE views >= 1000 AND title = 'SQL 인덱스 완벽 정리';
```

결과
![result](/assets/img/multiplecolumntitleviewscheck2.png)

- 결과가 동일한 것을 알 수 있음.
- 중요한 것은 인덱스의 첫 번째 컬럼에는 동등 비교(`=`) 조건을, 마지막 컬럼에는 범위 조건(`>`, `<` 등)을 사용하는 것이 가장 효율적임.

실패 예시 1
```sql
-- 인덱스 생성 구문 --
CREATE INDEX idx_articles_category_views ON articles (category, views);

EXPLAIN SELECT * FROM articles WHERE category >= 'Frontend' AND views = 1100;
```

결과
![result](/assets/img/multiplecolumnindexfail1.png)

- 언뜻보면 인덱스를 잘 탄 것 같지만 filtered 항목을 보면 뭔가 이상함.
  - 10.00으로 표시되어 있는 것을 확인할 수 있음.
    - 옵티마이저가 예상하기로 인덱스 범위로 읽은 레코드 중 10%만 실제 `WHERE` 조건을 만족한다는 뜻임.
      - 인덱스가 범위를 넓게 읽고 내부적으로 추가 필터링을 수행하고 있다는 의미임.
    - 이는 인덱스 첫 번째 컬럼에 범위 검색을, 두번째 컬럼에 동등비교를 했기 때문에 발생한 문제임.
      - 첫 번째 컬럼인 `category`를 기준으로 정렬되고, 같은 `category` 값을 가진 데이터를 기준으로 다시 `views`를 정렬시키는데 조건을 위 예시처럼 사용할 경우 `category`는 인덱스를 타지만 `views`는 제대로 정렬되지 않았기 때문에 인덱스를 타지 못하고 추가 필터링 작업이 생긴 것임.
        - 이때 MySQL은 인덱스에 저장된 (`category`, `views`) 쌍을 훑으면서 `views` 조건을 걸러냄.
          - 인덱스 내부에서 조건 검증이 일어나고, 이 단계에서는 아직 테이블의 실제 데이터 페이지를 읽지 않음.

실패 예시 2
```sql
-- 인덱스 생성 구문 --
CREATE INDEX idx_articles_category_views ON articles (category, views);

EXPLAIN SELECT * FROM articles WHERE views = 1100;
```

결과
![result](/assets/img/multiplecolumnindexfail2.png)

- `idx_articles_category_views` 인덱스를 타지 못하고 풀 테이블 스캔이 일어난 것을 확인할 수 있음.
- 생성한 인덱스는 `category` 컬럼의 데이터로 먼저 정렬한 후 동일한 데이터 내에서 다시 `views`를 정렬하기 때문에 위 예시처럼 `WHERE` 조건을 사용하게 되면 제대로 인덱스를 탈 수 없음.

### **IN절 활용**
실패 예시 1
```sql
-- 인덱스 생성 구문 --
CREATE INDEX idx_articles_category_views ON articles (category, views);

EXPLAIN SELECT * FROM articles WHERE category >= 'Frontend' AND views = 1100;
```

- 위 쿼리는 잘못된 범위 검색과 동등 비교 사용으로 인해 제대로 인덱스를 타지 못하는 쿼리임.
- 이것을 `IN` 절을 사용해서 처리하는게 가능함.

`IN`절 활용 예시
```sql
-- 인덱스 생성 구문 --
CREATE INDEX idx_articles_category_views ON articles (category, views);

EXPLAIN SELECT * FROM articles WHERE category IN ('Frontend', 'Programming') AND views = 1100;
```

결과
![result](/assets/img/multiplecolumnindexinuse.png)

- 결과를 확인해보면 제대로 인덱스를 탄 것을 확인할 수 있음.
- MySQL 옵티마이저는 `IN`절을 여러 개의 동등 비교(`=`)로 확장해서 처리함.
  - 인덱스의 첫 번째 컬럼(`category`)에 동등 조건이 적용된 것과 동일하게 작동함.

### **복합 인덱스 정렬**
- `WHERE` 절의 필터링과 `ORDER BY`의 정렬 방향이 인덱스 순서와 일치하면 DB는 filesort 작업을 생략해서 성능을 향상시킬 수 있음.

예시
```sql
-- 인덱스 생성 구문 --
CREATE INDEX idx_articles_category_views ON articles (category, views);

EXPLAIN SELECT * FROM articles WHERE category = 'Database' AND views >= 500 ORDER BY views;
```

결과
![result](/assets/img/multiplecolumnindexorderbycheck.png)

- filesort가 생략된 것을 확인할 수 있음.
  - 인덱스 순서와 정렬 순서가 일치할 경우 MySQL은 별도의 정렬 과정(filesort)을 수행하지 않음.