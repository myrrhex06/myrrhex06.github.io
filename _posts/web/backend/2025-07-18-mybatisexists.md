---
title: Exists 쿼리 작성 방법에 대해 알아보자.
date: 2025-07-16 21:30:00 +0900
categories: [Language, SQL]
tags: [sql, mysql]
---

Spring Data JPA를 사용하면 자동으로 쿼리를 만들어주는 기능이 있어서 보통은 그걸 사용해서 `Exists` 기능을 사용했었다.

하지만, Mybatis에 대해서 공부도 할겸 사이드 프로젝트를 진행하면서 모든 쿼리를 직접 xml 파일에 작성하기 시작했고 ORM에 가려진 쿼리에 대한 이해 부족 문제가 드러나기 시작했다.

오늘은 그 문제들 중 극히 일부분인 `Exists` 쿼리에 대해 설명하겠다.

> 이 블로그 글의 설명은 MySQL을 기준으로 설명한다.

## **Exists 쿼리**
워낙 간단하고 이해하기 쉽기 때문에 공부랄 것도 없이 예전에 사용했던 기억을 생각해서 사용하였다.

`Exists`는 아래와 같은 구조를 띈다.

```sql
SELECT EXISTS(
  서브쿼리
)
```

서브쿼리에 데이터가 1건이라도 존재한다면 1, 없으면 0을 리턴하는 굉장히 간단한 쿼리이다.

사용 예시

```sql
SELECT EXISTS(
  SELECT 1 FROM USER_INFO u WHERE u.nickname = 'test'
)
```

`SELECT 1`을 설정한 이유는 서브 쿼리에서 사용되는 SELECT 절에서 불필요한 컬럼 조회를 막기 위해서이다.