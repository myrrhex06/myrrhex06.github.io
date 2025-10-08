---
title: "인덱스란?"
date: 2025-10-08 13:14:00 +0900
categories: [Language, SQL]
tags: [sql, mysql, index, select]
---

## **인덱스란?**
- 특정 컬럼들의 데이터를 기반으로 생성되는 원본 테이블과는 별개의 특수한 자료 구조임.
- 지정된 컬럼의 값과 해당 값을 가진 실제 데이터 행의 위치(`PK`, 포인터 등)를 한 쌍으로 저장함.
- 인덱스 내부의 데이터는 항상 정렬된 상태를 유지함.
  - B-Tree 알고리즘을 사용함.
- 인덱스가 붙어 있으면 풀 테이블 스캔이 일어나지 않고 인덱스를 타서 조회하여 빠르게 조회 처리가 가능함.

> 풀 테이블 스캔(Full Table Scan)이란 테이블 전체 행(row)을 스캔하는 것을 의미함.
{: .prompt-tip }


### **인덱스 유형**
- **클러스터 인덱스(Clustered Index)**: 기본키(`PK`)를 기반으로 만드는 인덱스임.
  - 기본키(`PK`) 자체가 인덱스 역할을 수행하는 것임.
- **보조 인덱스(Secondary Index)**: 원본 데이터의 기본키(`PK`) 값을 함께 보관함.
  - 기본키(`PK`) 값으로 클러스터 인덱스를 통해 원하는 데이터를 조회함.

### **인덱스 생성**
- MySQL은 `PK`, `FK`, `Unique Key`에 대한 인덱스를 자동으로 만들어줌.

**Unique Key에 인덱스를 생성하는 이유**
- 새로운 데이터를 삽입하거나 기존 데이터를 수정할 때마다 입력하려는 값이 테이블에 이미 존재하는지 빠르게 확인해야하기 때문임.
  - 만약 인덱스가 없을 경우 중복 검사를 위해 매번 풀 테이블 스캔이 발생하기 때문에 쓰기 성능이 떨어질 수 있음.

구문
```sql
CREATE INDEX index_name ON table_name (column_name1, ...);
```

예시
```sql
CREATE INDEX idx_articles_title ON articles (title);
```

### **인덱스 조회**
구문
```sql
SHOW INDEX FROM table_name;
```

예시
```sql
SHOW INDEX FROM articles;
```

결과
![result](/assets/img/showindexresult.png)

- `Key_name`: 인덱스 이름
- `Column_name`: 해당 인덱스가 어떤 컬럼을 기반으로 만들어졌는지를 표시함.
- `Non_unique`:
  - `1`: 중복 값을 허용하는 인덱스
  - `0`: 중복 값을 허용하지 않는 고유 인덱스
    - `UNIQUE` 또는 `PRIMARY` KEY라는 의미임.
- `Cardinality`:  인덱스에 저장된 고유한 값의 개수에 대한 추정치
  - 값이 높을수록 중복도가 낮다는 의미
    - 인덱스의 성능이 좋다고 판단 가능함.
    - 중복도가 높을수록 인덱스를 사용하는 의미가 없어짐.

> 결과를 확인해보면 `PRIMARY KEY`에 인덱스가 만들어져 있는 것을 확인할 수 있음.
{: .prompt-tip }

### **인덱스 제거**
구문
```sql
DROP INDEX index_name ON table_name;
```

예시
```sql
DROP INDEX idx_articles_title ON articles;
```