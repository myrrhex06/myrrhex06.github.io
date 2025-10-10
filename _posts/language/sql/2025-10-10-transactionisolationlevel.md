---
title: "트랜잭션의 격리 수준이란?"
date: 2025-10-10 09:45:00 +0900
categories: [Language, SQL]
tags: [sql, mysql, transaction, commit, rollback, dirtyread, nonrepeatableread, phantomread, readuncommitted, readcommitted, repeatableread, serializable]
---

## **동시성의 문제**
트랜잭션이 만약 격리성을 보장하지 않는다면 다양한 동시성 문제가 발생할 수 있음.

**대표적인 문제들**
- **더티 리드(Dirty Read)**: 한 트랜잭션이 아직 `COMMIT` 하지 않은 수정 중인 데이터를 다른 트랜잭션이 읽는 것을 의미함.
- **반복 불가능 읽기(Non-Repeatable Read)**: 한 트랜잭션에서 똑같은 `SELECT` 쿼리를 실행했을 때 그 사이에 다른 트랜잭션이 값을 수정하고 `COMMIT` 하여 두 `SELECT` 쿼리의 결과가 다르게 나오는 현상.
- **유령 읽기(Phantom Read)**: 한 트랜잭션 내에서 특정 범위의 데이터를 두 번 읽었는데 첫 번째 조회에서는 없었던 새로운 행이 두번째 행에서 나타나는 현상.
  - 다른 트랜잭션이 중간에 새로운 행을 `INSERT`하고 `COMMIT`한거임.

> 반복 불가능 읽기와 유령 읽기는 사실 많이 발생하는 문제는 아님. 한 트랜잭션 내에서 같은 `SELECT` 쿼리를 여러 번 날리는 경우는 거의 없기 때문임.
{: .prompt-tip }

## **트랜잭션의 격리 수준**
이런 동시성 문제들을 해결하기 위해 트랜잭션은 격리성을 보장하며, 격리 수준을 조정하여 격리 수준을 높이거나, 낮출 수 있음.

| 격리 수준 (Isolation Level) | 더티 리드 (Dirty Read) | 반복 불가능 읽기 (Non-Repeatable Read) | 유령 읽기 (Phantom Read) |
| --------------------------- | ---------------------- | -------------------------------------- | ------------------------ |
| `READ UNCOMMITTED`          | 발생                   | 발생                                   | 발생                     |
| `READ COMMITTED`            | 방지                   | 발생                                   | 발생                     |
| `REPEATABLE READ`           | 방지                   | 방지                                   | 발생                     |
| `SERIALIZABLE`              | 방지                   | 방지                                   | 방지                     |

- `READ UNCOMMITTED`: 가장 낮은 격리 수준.
  - 정합성 이슈가 매우 많기 때문에 잘 사용되지 않음.
- `READ COMMITTED`: `COMMIT`된 데이터만 읽을 수 있음.
  - 더티 리드 방지 가능함.
- `REPEATABLE READ`: 한 트랜잭션 내에서 데이터의 일관된 조회를 보장해줌.
  - MySQL의 InnoDB 스토리지 엔진이 기본으로 사용하는 격리 수준임
- `SERIALIZABLE`: 가장 높은 격리 수준.
  - 동시성 문제를 완벽하게 차단함.
  - 모든 트랜잭션이 직렬적으로 실행되기 때문에 성능은 포기해야함.

> 대부분 서비스에서는 `READ COMMITTED`나 `REPEATABLE READ` 수준을 사용함. <br>
> 높은 격리 수준일수록 안전하지만, 성능 저하가 크기 때문에 상황에 맞게 선택해야 함.
{: .prompt-tip }

### **격리 수준 확인 및 변경**
격리 수준 확인 구문
```sql
SELECT @@transaction_isolation;
```

**격리 수준 변경**
- 격리 수준은 특정 트랜잭션에만 적용하거나 현재 세션 내내 적용하거나, 데이터베이스 시스템 전체에 적용 가능함.
  - 현재 세션에서만 적용하는게 가장 일반적으로 사용됨.

> 세션이란 데이터베이스 클라이언트(ex. MySQL Workbench, DBeaver 등)가 데이터베이스 서버에 연결된 순간부터 연결을 종료할 때까지의 논리적인 연결 단위를 의미함.
{: .prompt-tip }

현재 세션 격리 수준 변경 구문
```sql
SET SESSION TRANSACTION ISOLATION LEVEL isolation_level;
```

예시
```sql
SET SESSION TRANSACTION ISOLATION LEVEL READ COMMITTED;
```

데이터베이스 시스템 전체 적용 구문
```sql
SET GLOBAL TRANSACTION ISOLATION LEVEL isolation_level;
```

- 연결되는 모든 세션의 격리 수준이 변경되기 때문에 주의해서 사용해야함.

> DB 서버를 아예 껐다 킬 경우 `SET GLOBAL` 설정이 날아가기 때문에 영구적으로 설정하기 위해선 MySQL 설정 파일을 변경해야함.
{: .prompt-tip }

예시
```sql
SET GLOBAL TRANSACTION ISOLATION LEVEL READ COMMITTED;
```