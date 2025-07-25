---
title: Spring JPA GeneratedValue SEQUENCE에 대해 알아보자.
date: 2025-07-25 21:40:00 +0900
categories: [Web, Backend]
tags: [backend, spring, jpa, orm, sequence]
---

오늘은 어제 올린 `IDENTITY` 블로그 글에 이어서 `SEQUENCE`에 대해 마저 알아보자.

> [GeneratedValue IDENTITY 블로그 글 참고](/posts/springjpaidentity/)

## **`SEQUENCE`**
SEQUENCE는 주로 PostgreSQL, Oracle과 같이 AUTO_INCREMENT가 지원되지 않는 DB에서 사용된다.

> MariaDB 10.3 버전 이후부터는 MariaDB도 시퀀스 사용이 가능하다.

### **`SEQUENCE` 동작 구조**
JPA에서 `SEQUENCE` 키 생성 전략을 사용할 경우 동작 구조는 아래와 같다.

![jpaSequence](/assets/img/jap_sequence_process.png)

1. `save()` 메서드에 인자값으로 `Entity` 클래스를 넘겨 호출한다.
2. JPA에서는 이때 `IDENTITY` 동작 구조와는 다르게 DB에 `SEQUENCE` 값을 요청하는 쿼리를 먼저 실행시킨다.
3. DB에서 `SEQUENCE` 값을 반환한다.
4. 반환된 `SEQUENCE` 값을 `Entity` PK 필드에 할당 후 `INSERT` 쿼리를 만들어 실행시킨다.

JPA에서 DB에 날리는 시퀀스 조회 쿼리는 아래와 같은 형태를 가진다.

```sql
SELECT nextval('시퀀스명');
```

### **`SEQUENCE` 설정 예시**
예시
```java
@Entity
@SequenceGenerator(
  name = "seq_example_generator",
  sequenceName = "seq_example",
  initialValue = 1,
  allocationSize = 1
)
public class ExampleEntity{
  @Id
  @GeneratedValue(strategy = GenerationType.SEQUENCE, generator = "seq_example_generator")
  private Long id;
}
```

`IDENTITY`와는 다르게 `@SequenceGenerator` 어노테이션을 사용해서 시퀀스 구성을 직접 해줘야한다.

이떄 사용되는 옵션들은 아래와 같다.
- `name`: JPA에서 사용되는 `@SequenceGenerator` 이름
- `sequenceName`: DB 시퀀스 이름
- `initialValue`: 시퀀스 시작값
- `allocationSize`: 메모리 캐싱 개수

여기서 자세하게 살펴봐야할 옵션값은 `allocationSize`이다.<br>
해당 옵션은 메모리 캐싱 개수를 지정하는 옵션이다.

예를 들어 `allocationSize`를 50으로 설정해둘 경우 아래와 같이 동작한다.

```sql
SELECT nextval('시퀀스명'); 
```
1. 위 쿼리를 실행할 때 시퀀스 값을 50개를 미리 가져온다.
2. `save()` 메서드가 다시 호출되면 또 시퀀스 값 조회 쿼리를 실행시키지 않고 내부적으로 캐싱되어 있는 시퀀스값들을 할당해준다.

이러한 동작 방식 덕분에 `IDENTITY` 방식보다 성능, 배치 처리에 더욱 유용하다.

하지만, `IDENTITY` 보다 초기 설정이 번거롭고 DB 시퀀스 객체도 관리해야하기 때문에 번거로운 작업이 생길 수 있다.