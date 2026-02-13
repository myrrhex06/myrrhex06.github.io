---
title: "SQL - 커버링 인덱스란?"
date: 2025-10-08 20:21:00 +0900
categories: [Language, SQL]
tags: [sql, mysql, index, select]
---

## **커버링 인덱스란?**
- 쿼리에 필요한 모든 컬럼을 포함하고 있는 인덱스를 뜻함.
  - 랜덤 I/O를 줄여 성능을 향상시킴.

> 기본적으로 보조 인덱스는 해당 컬럼의 데이터와 기본키(`PK`) 값을 한 쌍으로 저장하여 인덱스를 탈 경우 컬럼의 데이터와 매핑되어 있는 기본키값을 통해 원본 데이터를 조회하게 되는데, 이렇게 인덱스에 저장되어 있는 기본키값을 통해 원본 테이블을 다시 조회하는 것을 랜덤 I/O라고 함.
{: .prompt-tip }

일반 인덱스 예시
```sql
-- 인덱스 생성 --
CREATE INDEX idx_articles_title ON articles (title);

SELECT article_id, title, views FROM articles WHERE title = 'SQL 인덱스 완벽 정리';
```

- 위 쿼리의 경우 `idx_articles_title` 인덱스에는 `title` 컬럼의 데이터와 기본키(`article_id`) 값이 매핑되어 한쌍으로 저장됨.
- 인덱스를 타긴 하지만, `views` 컬럼은 인덱스에 포함되어 있지 않기 때문에 기본키값을 통해 원본 테이블을 조회하는 랜덤 I/O 과정이 필요함.

커버링 인덱스 예시
```sql
SELECT article_id, title FROM articles WHERE title = 'SQL 인덱스 완벽 정리';
```

- 위 쿼리의 경우 `idx_articles_title` 인덱스에 포함되어 있는 컬럼들만을 사용하기 때문에 조회 시 다시 원본 테이블을 조회하는 과정인 랜덤 I/O 과정이 생략됨.
  - 인덱스 내부에 조회되는 컬럼의 값이 모두 포함되어 있기 때문임.

만약 인덱스에 포함되지 않은 컬럼을 빠른 성능으로 조회해야 한다면, 해당 컬럼이 추가된 새로운 인덱스를 만들어 사용해야함.

커버링 인덱스 적용 및 실행 계획 확인 예시
```sql
CREATE INDEX idx_articles_title_views ON articles (title, views);

EXPLAIN SELECT article_id, title, views FROM articles WHERE title = 'SQL 인덱스 완벽 정리';
```

결과
![result](/assets/img/coveringindexcheck.png)

- Extra에 Using index를 확인할 수 있음.
  - 커버링 인덱스를 사용했다는 뜻임.

### **커버링 인덱스의 장/단점**
**장점**
- **`SELECT` 성능 향상**: 랜덤 I/O를 제거하기 때문에 성능이 개선됨.

**단점**
- **저장 공간 증가**: 인덱스는 원본 데이터와 별도의 저장 공간을 차지하는데, 인덱스에 포함된 컬럼이 많아질수록 인덱스의 크기도 커짐.
- **쓰기 성능 저하**: 쓰기 작업(`INSERT`, `UPDATE`, `DELETE`) 시 테이블 데이터뿐만 아니라 인덱스도 함께 수정해야함.
  - 인덱스가 많고 복잡할수록 쓰기 작업에 대한 부하가 커짐.