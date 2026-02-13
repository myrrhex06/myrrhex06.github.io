---
title: Backend - Spring JPA GeneratedValue IDENTITY에 대해 알아보자.
date: 2025-07-24 21:40:00 +0900
categories: [Web, Backend]
tags: [backend, spring, jpa, orm, autoincrement, sequence]
---

Spring Data JPA를 사용하여 PK에 자동 값 생성을 지정하는 방법으로 가장 간단한 것은 `@GeneratedValue` 어노테이션을 사용하는 것이다.

오늘은 사용법도 간단하고, 많이 사용되는 자동 키 생성 전략인 `IDENTITY`에 대해서 알아보자.

## **`IDENTITY`**
`IDENTITY`는 주로 MySQL같은 `AUTO_INCREMENT`를 지원하는 DB에 사용된다.

### **`IDENTITY` 동작 구조**
`IDENTITY` 전략은 `ID` 값을 JPA가 생성하는 게 아니라 DB에서 직접 생성되기 때문에, JPA의 일반적인 `persist`/`flush` 흐름과 차이가 있다. 

아래는 그 동작 구조다.

![identityProcess](/assets/img/jpa_identity_process.png)

1. `Repository`에 `save()` 메서드 호출
2. `Entity` 객체의 값을 기반으로 `INSERT` 쿼리를 만들어 실행하는데, 이때 `IDENTITY` 전략이 적용된 PK 필드는 DB에서 처리되기 때문에 쿼리에서 제외되거나 `null`로 처리
3. DB에서 `AUTO_INCREMENT` 처리 되어 PK 생성 후 데이터 `INSERT`
4. `JDBC`를 통해 저장된 PK 값 JPA로 반환
5. DB에서 넘어온 PK 값을 `Entity`에 `IDENTITY`로 지정된 필드에 할당 후 사용자에게 리턴

이때, `INSERT` 쿼리를 실행하는 시점은 `EntityManager`의 `persist()` 메서드가 실행되는 시점에 `INSERT` 쿼리가 실행된다.

`Repository`에 `save()` 메서드 구현을 살펴보면 `persist()` 메서드를 사용하는 것을 볼 수 있다.

`SimpleJpaRepository save()` 메서드 구현
```java
@Transactional
public <S extends T> S save(S entity) {
  Assert.notNull(entity, "Entity must not be null");
  if (this.entityInformation.isNew(entity)) {
    this.entityManager.persist(entity); // INSERT 쿼리 실행
    return entity;
  } else {
    return this.entityManager.merge(entity);
  }
}
```

> `SimpleJpaRepository`는 `JpaRepository`의 구현체이다.
{: .prompt-tip }

JPA에서 `persist()` 메서드는 원래 영속성 컨텍스트에 `Entity`를 등록하는 역할을 하며, `flush()` 메서드를 사용하여 영속성 컨텍스트에 있는 변경사항을 반영하는 방식을 사용한다. 

하지만, `IDENTITY` 전략을 사용할 경우 PK 값 처리를 DB에서 진행하기 때문에 `persist()`를 하자마자 바로 INSERT 쿼리를 날리고 생성된 ID 값을 가져와서 Entity에 채워넣는 식으로 동작한다. 

### **`IDENTITY` 설정 예시**
`ExampleEntity.java`
```java
@Entity
public class ExampleEntity{
  @Id
  @GeneratedValue(strategy = GenerationType.IDENTITY)
  private Long id;
}
```