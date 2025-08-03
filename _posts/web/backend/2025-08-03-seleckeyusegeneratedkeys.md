---
title: "Mybatis useGeneratedKeys, selectKey에 대해서 알아보자."
date: 2025-08-03 10:40:00 +0900
categories: [Web, Backend]
tags: [backend, spring, database, mybatis, sql]
---

Mybatis를 사용하여 `INSERT` 쿼리를 날릴 때, JPA와는 다르게 저장된 객체를 별도로 반환해주지 않는다.

따라서, Mybatis에서는 DB에 저장된 객체에 자동으로 생성된 ID 값을 알고 싶으면 `useGeneratedKeys` 또는 `selectKey를` 사용해야한다.

## **useGeneratedKeys**
`useGeneratedKeys`는 자동 증가(`AUTO_INCREMENT`) 컬럼의 값을 Mybatis가 직접 JDBC를 사용하여 받아오도록 설정한다.

주로 아래와 같은 상황에서 사용한다.
- Mybatis(`AUTO_INCREMENT`), MSSQL(`IDENTITY`)와 같은 자동 증가 키를 사용할 때
- `INSERT` 수행 후 생성된 키를 바로 받아서 객체에 할당할 때


### **사용 방법**

`INSERT` 태그의 속성으로 `useGeneratedKeys`, `keyProperty`라는 속성을 지정해주면 된다.

예시
```xml
<insert id="insertUser" parameterType="User" useGeneratedKeys="true" keyProperty="userSeq">
    INSERT INTO users (email, password)
    VALUES (#{email}, #{password})
</insert>
```

- `userSeq`는 `User` 객체의 있는 기본키에 매핑되는 필드명
- `INSERT` 끝나면 자동으로 생성된 PK 값이 `userSeq`에 들어감

### **내부 동작 방식**

`useGeneratedKeys`를 사용하게 되면, Mybatis에서는 아래와 같은 구조로 동작하게 된다.

1. `<insert useGeneratedKeys="true" keyProperty="id" />` 사용
2. MyBatis가 `Jdbc3KeyGenerator`라는 클래스에 `processAfter` 메서드를 호출
3. `keyProperty`에 설정된 필드에 PK값 할당


`Jdbc3KeyGenerator`에 구현되어 있는 `processAfter` 메서드는 아래와 같다.

`Jdsb3KeyGenerator.java`
```java
public void processAfter(Executor executor, MappedStatement ms, Statement stmt, Object parameter) {
    this.processBatch(ms, stmt, parameter);
}

public void processBatch(MappedStatement ms, Statement stmt, Object parameter) {
    String[] keyProperties = ms.getKeyProperties();

    if (keyProperties != null && keyProperties.length != 0) {
        try {
            ResultSet rs = stmt.getGeneratedKeys();

            try {
                ResultSetMetaData rsmd = rs.getMetaData();
                Configuration configuration = ms.getConfiguration();

                if (rsmd.getColumnCount() >= keyProperties.length) {
                    this.assignKeys(configuration, rs, rsmd, keyProperties, parameter);
                }
            } catch (Throwable var9) {
                if (rs != null) {
                    try {
                        rs.close();
                    } catch (Throwable var8) {
                        var9.addSuppressed(var8);
                    }
                }

                throw var9;
            }

            if (rs != null) {
                rs.close();
            }

        } catch (Exception var10) {
            Exception e = var10;
            throw new ExecutorException("Error getting generated key or setting result to parameter object. Cause: " + e, e);
        }
    }
}
```

위 코드와 같이 내부적으로 `processAfter` 메서드가 호출되면 `proccessBatch` 메서드가 실행되고, `getGeneratedKeys` 메서드에서 생성된 PK 값을 `ResultSet`에 담는 것을 확인할 수 있다.

## **selectKey**
`selectKey`는 값을 미리 또는 나중에 조회해서 조회된 값을 객체에 할당하는 역할을 한다.

즉, DB에서 PK를 생성하는 것이 아니라 `SELECT`문을 직접 만들어서 `INSERT` 전후에 값을 넣어주는 것이다.

주로 아래와 같은 상황에서 사용한다.
- `SEQUENCE`를 통해 PK를 생성하는 DB(Oracle, PostgreSQL)를 사용할 때

**사용 방법**

`INSERT` 태그 안에 `selectKey`라는 태그를 넣고 속성을 설정하여 사용이 가능하다.

사용 예시
```xml
<insert id="insertUser" parameterType="User">
    
    <selectKey keyProperty="id" resultType="int" order="BEFORE">
        SELECT USER_SEQ.NEXTVAL FROM DUAL
    </selectKey>
    
    INSERT INTO users (id, name, email)
    VALUES (#{id}, #{name}, #{email})

</insert>
```

- `keyProperty`: 조회 결과를 매핑할 필드 이름
- `resultType`: `SELECT`로 조회한 결과의 타입
- `order`: `BEFORE` 또는 `AFTER` 사용 가능
  - `BEFORE`: `INSERT` 전의 실행
  - `AFTER`: `INSERT` 후의 실행

### **BEFORE vs AFTER**
**BEFORE**
- `INSERT` 하기 전에 키를 구함
- `SEQUENCE` 기반(Oracle, PostgreSQL)에 적합한 방식

예시
```xml
<selectKey keyProperty="id" resultType="int" order="BEFORE">
    SELECT USER_SEQ.NEXTVAL FROM DUAL
</selectKey>
```

**AFTER**
- `INSERT` 후에 키를 구함
- DB가 자동으로 키 값을 생성해주는 경우(보통 `AUTO_INCREMENT`)
  - 이 경우에는 `selectKey`보단 `useGenerated` 사용이 일반적

예시
```xml
<selectKey keyProperty="id" resultType="int" order="AFTER">
    SELECT LAST_INSERT_ID()
</selectKey>
```

## **내부 동작 과정**
1. Mybatis가 `INSERT` 쿼리를 실행할 때 `MappedStatment`를 통해 `selectKey` 쿼리를 먼저(또는 나중에) 실행 
2. 반환된 결과는 Mybatis의 `KeyGenerator` 인터페이스를 구현한 `SelectKeyGenerator` 클래스가 받아서 처리

> `SelectKeyGenerator`가 반환된 키를 파라미터 객체의 `keyProperty`에 집어넣는 역할

`SelectKeyGenerator` 내에 구현된 코드는 아래와 같다.

`SelectKeyGenerator.java`
```java
public void processBefore(Executor executor, MappedStatement ms, Statement stmt, Object parameter) {
    if (this.executeBefore) {
        this.processGeneratedKeys(executor, ms, parameter);
    }
}

public void processAfter(Executor executor, MappedStatement ms, Statement stmt, Object parameter) {
  if (!this.executeBefore) {
    this.processGeneratedKeys(executor, ms, parameter);
  }
}

private void processGeneratedKeys(Executor executor, MappedStatement ms, Object parameter) {
    try {
        if (parameter != null && this.keyStatement != null && this.keyStatement.getKeyProperties() != null) {
            String[] keyProperties = this.keyStatement.getKeyProperties();
            Configuration configuration = ms.getConfiguration();
            MetaObject metaParam = configuration.newMetaObject(parameter);
            Executor keyExecutor = configuration.newExecutor(executor.getTransaction(), ExecutorType.SIMPLE);
            List<Object> values = keyExecutor.query(this.keyStatement, parameter, RowBounds.DEFAULT, Executor.NO_RESULT_HANDLER);

            if (values.isEmpty()) {
                throw new ExecutorException("SelectKey returned no data.");
            }

            if (values.size() > 1) {
                throw new ExecutorException("SelectKey returned more than one value.");
            }

            MetaObject metaResult = configuration.newMetaObject(values.get(0));

            if (keyProperties.length == 1) {
                if (metaResult.hasGetter(keyProperties[0])) {
                    this.setValue(metaParam, keyProperties[0], metaResult.getValue(keyProperties[0]));
                } else {
                    this.setValue(metaParam, keyProperties[0], values.get(0));
                }
            } else {
                this.handleMultipleProperties(keyProperties, metaParam, metaResult);
            }
        }

    } catch (ExecutorException var10) {
        ExecutorException e = var10;
        throw e;
    } catch (Exception var11) {
        Exception e = var11;
        throw new ExecutorException("Error selecting key or setting result to parameter object. Cause: " + e, e);
    }
}
```

- `BEFORE`일 경우 `processBefore` 메서드 실행
- `AFTER`일 경우 `processAfter` 메서드 실행

```java
List<Object> values = keyExecutor.query(this.keyStatement, parameter, RowBounds.DEFAULT, Executor.NO_RESULT_HANDLER);
```

위 코드에서 `selectKey` 태그에 작성한 쿼리가 실행되어 PK 값을 조회한다.

