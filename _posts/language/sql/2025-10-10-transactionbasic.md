---
title: "SQL - 트랜잭션이란?"
date: 2025-10-10 09:28:00 +0900
categories: [Language, SQL]
tags: [sql, mysql, transaction, commit, rollback]
---

## **트랜잭션이란?**
- 여러 작업을 하나의 논리적인 단위로 묶은 것을 의미함.
  - 트랜잭션 내 모든 작업이 모두 성공하면 반영(`COMMIT`)
  - 하나라도 실패하면 전부 취소(`ROLLBACK`)
- 작업의 신뢰성을 높여줌.

### **ACID 속성**
- 트랜잭션이 신뢰성을 갖추기 위해 반드시 지켜야하는 4가지 속성.
- **Atomicity (원자성)**: 트랜잭션은 더 이상 나눠질 수 없는 논리적 단위이며 모두 성공하거나 모두 실패함.
  - 모두 성공: `COMMIT`
  - 모두 실패: `ROLLBACK`
- **Consistency (일관성)**: 트랜잭션의 실행 결과가 DB에 설정된 모든 규칙(ex. 제약 조건 등)을 위반하지 않음을 보장하는 속성임.
  - 결과의 유효성을 보장함.
- **Isolation (격리성)**: 하나의 트랜잭션이 실행중일 때 다른 트랜잭션이 해당 트랜잭션의 중간 결과에 끼어들어 간섭할 수 없도록 격리함.
  - 격리성은 어느 정도로 격리시킬건지에 따라 격리 수준이라는 몇 가지의 단계로 나뉨.
    - 격리 수준을 높일 경우 데이터는 안전하지만 성능이 저하됨.
    - 격리 수준을 낮출 경우 성능은 향상되지만 데이터의 안전성은 떨어짐.
- **Durability (지속성)**: `COMMIT`된 트랜잭션 결과는 영구적으로 DB에 반영되도록 보장하는 속성

### **트랜잭션 구문**
- `START TRANSACTION`: 트랜잭션 시작을 선언하는 커맨드
  - 이 명령어 후 실행되는 모든 쿼리는 하나의 트랜잭션에 묶여 관리됨.
  - `BEGIN` 으로 사용되기도 함.
- `COMMIT`: 트랜잭션 내의 작업이 모두 완료되었고, DB에 영구적으로 반영하라는 의미.
  - `COMMIT` 후에는 변경 내용을 되돌릴 수 없음.
- `ROLLBACK`:  문제가 발생했으니 다시 원본 데이터로 복구하라는 의미.

`COMMIT` 예시
```sql
START TRANSACTION; -- 트랜잭션 시작 --

UPDATE articles 
SET views = views + 1
WHERE id = 1;

COMMIT; -- 변경 사항 영구 반영 --
```

`ROLLBACK` 예시
```sql
START TRANSACTION; -- 트랜잭션 시작 --

UPDATE articles 
SET views = views + 1
WHERE id = 1;

ROLLBACK; -- 원본 데이터 복구 --
```
