---
title: "SQL - CASE문이란?"
date: 2025-10-07 15:30:00 +0900
categories: [Language, SQL]
tags: [sql, mysql, case, select, when, then, else, end]
---

## **CASE문이란?**
- 특정 데이터를 조건에 맞게 동적으로 가공해줌.
- 프로그래밍에서 `if-else`문과 같은 역할을 수행함.

`CASE`문은 아래와 같이 두 가지 유형으로 나뉨.
- 단순 `CASE`문
- 검색 `CASE`문

### **단순 CASE문**
- 특정 하나의 컬럼이나 표현식의 값에 따라 결과를 다르게 하고 싶을 때 사용함.

구문
```sql
CASE column_or_expression
  WHEN value1 THEN result1
  WHEN value2 THEN result2
  ...
  ELSE default_value
END
```

> `ELSE`를 생략한 경우에 `WHEN` 절에 일치하는 값이 없을 경우 `NULL`이 반환됨
{: .prompt-tip }

예시
```sql
SELECT 
  CASE status
    WHEN 'PENDING' THEN '대기중'
    WHEN 'PROGRESS' THEN '진행중'
    WHEN 'COMPLETED' THEN '완료'
    ELSE '실패'
  END AS send_status
FROM notifications;
```

### **검색 CASE문**
- `WHEN` 절에 조건을 넣어서 처리함.

구문
```sql
CASE
  WHEN conditional1 THEN result1
  WHEN conditional2 THEN result2
  ...
  ELSE default_value
END
```

예시
```sql
SELECT 
  CASE
    WHEN views >= 3000 THEN '인기글'
    WHEN views >= 1000 THEN '떠오르는 글'
    ELSE '일반'
  END AS descriptions
FROM articles;
```

### **CASE문 ORDER BY 활용**
- `CASE`문은 `ORDER BY`에서도 활용이 가능함.

`ORDER BY` 활용 예시
```sql
SELECT 
  CASE
    WHEN views >= 3000 THEN '인기글'
    WHEN views >= 1000 THEN '떠오르는 글'
    ELSE '일반'
  END AS descriptions
FROM articles
ORDER BY
  CASE
    WHEN views >= 3000 THEN 2
    WHEN views >= 1000 THEN 1
    ELSE 0
  END DESC
```

### **CASE문 GROUP BY 활용**
- `CASE`문은 `GROUP BY`를 통해 그룹핑을 할 때도 활용이 가능함.

예시
```sql
SELECT 
  CASE
    WHEN YEAR(registered_at) = 2000 THEN '2000년 가입'
    WHEN YEAR(registered_at) = 2002 THEN '2002년 가입'
    ELSE '필요 없음'
  END AS vip_date,
  COUNT(*) AS vip_count
FROM users
GROUP BY 
  CASE
    WHEN YEAR(registered_at) = 2000 THEN '2000년 가입'
    WHEN YEAR(registered_at) = 2002 THEN '2002년 가입'
    ELSE '필요 없음'
  END;
```

이때 `SELECT` 절에서 지정한 별칭을 `GROUP BY`에서 사용이 가능함.

> 일반적으로 `GROUP BY` 절은 `SELECT` 절보다 먼저 실행되기 때문에,
`SELECT`에서 지정한 별칭을 바로 사용할 수 없음.<br>
하지만 MySQL은 내부적으로 이를 허용하기 때문에 예외적으로 별칭 사용이 가능함.
{: .prompt-tip }

별칭 사용 예시 
```sql
SELECT 
  CASE
    WHEN YEAR(registered_at) = 2000 THEN '2000년 가입'
    WHEN YEAR(registered_at) = 2002 THEN '2002년 가입'
    ELSE '필요 없음'
  END AS vip_date,
  COUNT(*) AS vip_count
FROM users
GROUP BY vip_date;
```

### **CASE문 집계 함수 활용**
- 집계 함수(`SUM`, `AVG`) 내부에도 `CASE`문 활용이 가능함.

예시
```sql
SELECT 
  COUNT(*) AS total_send,
  SUM(CASE WHEN status = 'COMPLETED' THEN 1 ELSE 0 END) AS completed_count,
  SUM(CASE WHEN status = 'PROGRESS' THEN 1 ELSE 0 END) AS progress_count,
  SUM(CASE WHEN status = 'PENDING' THEN 1 ELSE 0 END) AS pending_count
FROM notifications;
```