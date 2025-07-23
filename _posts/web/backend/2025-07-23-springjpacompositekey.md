---
title: Spring Data JPA 복합키 설정에 대해 알아보자.
date: 2025-07-23 21:14:00 +0900
categories: [Web, Backend]
tags: [backend, spring, jpa, orm]
---

## **복합키란?**
복합키란 Primary Key를 하나가 아닌 두개 이상의 컬럼에 적용시키는 것을 뜻한다.

## **Spring Data JPA 복합키 설정**
JPA를 사용할 때 복합키를 설정하는 방식으로는 아래와 같이 2가지 방식이 존재한다.

- `@IdClass`
- `@EmbeddedId`

각 방식들에 설정 방법 및 장단점들에 대해 알아보자.

### **`@IdClass` 복합키 설정**
`@IdClass` 어노테이션을 사용할 경우 복합키 필드가 `Entity` 클래스에 그대로 존재하며, 복합키로 지정할 필드에 전부 `@Id` 어노테이션을 달아줘야한다.

예시

`ExampleEntity.java`
```java
@Entity
@IdClass(ExampleId.class)
public class ExampleEntity{
  @Id
  private String exId1;

  @Id
  private String exId2;
}
```

`ExampleId.java`
```java
public class ExampleId implements Serializable{
  private String exId1;
  private String exId2;

  @Override
  public boolean equals(Object o){
    ...
  }

  @Override
  public int hashCode(){
    ...
  }
}
```

`@IdClass` 어노테이션을 사용하여 복합키를 설정할 경우 PK로 사용되는 필드들을 하나의 독립적인 클래스로 만들어줘야한다.

위 예시에서는 `ExampleId` 라는 클래스를 만들어 `@IdClass` 어노테이션에 인자값으로 넘겨주었다.

이렇게 클래스를 넘겨주는 이유는 JPA에서 PK를 비교할 때 어노테이션에 인자값으로 넘긴 클래스를 사용하여 처리하기 때문이다.

> `@IdClass`에 인자값으로 사용되는 클래스는 PK를 비교할 때 사용되는 식별자 클래스이기 때문에 `equals`, `hashCode` 메서드를 구현해줘야만 한다.
{: .prompt-tip }

### **`@EmbeddedId` 복합키 설정**
`@EmbeddedId` 어노테이션을 사용하여 복합키를 설정할 경우 복합키 자체를 하나로 묶어서 관리한다.<br>

따라서, `Entity` 클래스 내부에는 `@EmbeddedId` 객체 하나만 존재하며 복합키에 적용되는 필드들은 모두 객체에 선언되어 있다.

예시

`ExampleEntity.java`
```java
@Entity
public class ExampleEntity{

	@EmbeddedId
	private ExampleId id;

}
```

`ExampleId.java`
```java
@Embeddable
public class ExampleId implements Serializable {
    private String ex1;
    private String ex2;
    
	  @Override
		public boolean equals(Object o){
			...
		}
		
		@Override
		public int hashCode(){
			...
		}
}
```

`@IdClass`를 사용했을 때와 마찬가지로 클래스 내부에는 `equals`, `hashCode`를 오버라이딩하여 구현해야한다.

### **`Serializable` 인터페이스를 구현하는 이유**
`@IdClass`, `@EmbeddedId` 두 방식 모두 `Serializable` 인터페이스를 구현하는 것을 확인할 수 있다.

`Serializable`을 구현하는 이유로는 아래와 같다.
- JPA가 Entity 식별자를 캐시에 넣을 때 Serializable이 없을 경우 에러가 발생할 수 있기 때문
- 서버가 여러 대일 경우 세션 클러스터링을 진행할 때 객체를 직렬화해야 하지만, `Serializable`을 구현하지 않았다면 직렬화에서 에러가 발생할 수 있기 때문

> 세션 클러스터링이란? 서버 A의 세션 정보를 서버 B, C, D 등 모든 서버에 복제해서 공유하는 것을 말한다.
{: .prompt-tip }

### **`@IdClass` VS `@EmbeddedId`**
**`@IdClass` 복합키 설정**

장점
- 기존 레거시 DB(PK 필드들이 엔티티 안에 바로 들어있을 때)와 호환성이 좋다.
- 코드 가독성이 `@EmbeddedId`에 비해 상대적으로 읽기 수월하다.

단점
- PK 정의가 `Entity` 클래스와 식별자 클래스에 모두 중복 선언되어야 한다.
- PK가 바뀔 경우 양쪽 모두 바꿔줘야하기 때문에 관리하기가 번거롭다.

<br>

**`@EmbeddedId` 복합키 설정**

장점
- PK 관련 필드는 `Entity`에서 분리되어 별도의 클래스에서 관리되기 때문에 관리 측면에서는 `@IdClass` 보다 뛰어나다.
- `Entity` 클래스 필드들이 단순하게 이루어진다.

단점
- 기존 레거시 DB와 바로 매칭할 때 손이 더 간다.
- 복합키에 접근할 때 코드가 좀 더 길어진다.
  - ex) `exampleEntity.id.ex1`