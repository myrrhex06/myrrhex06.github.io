---
title: "클래스, 객체, 인스턴스에 대해 알아보자."
date: 2025-08-06 21:30:00 +0900
categories: [CS]
tags: [class, instancem, object, oop]
---

## **클래스**
쉽게 말하면 객체를 만들기 위한 설계도이다.<br>
객체를 만들기 위한 상태(필드), 행위(메서드)만 정의되어 있다.

클래스 예시
```java
public class Book{
  private String title;
  private String content;
  private String isbn;

  // getter, setter
}
```

위와 같이 Book 이라는 클래스를 정의하였다.

이 Book 이라는 설계도를 토대로 객체를 만들 수 있게된다.

## **객체**
정의한 클래스(설계도)를 기반으로 만든 실체이다.<br>

객체 선언 예시
```java
public static void main(String args[]){
  Book book1;
}
```

## **인스턴스**
생성한 객체가 메모리에 할당된 상태를 뜻한다.

인스턴스 생성 예시
```java
public static void main(String args[]){
  Book book1 = new Book();
}
```

위 코드와 같이 `new` 키워드를 사용하여 객체를 메모리에 할당시킨 상태를 인스턴스라고 한다.

### **객체와 인스턴스의 차이**

- 객체: 정의한 클래스로 만든 실체(개념적으로 존재하는 상태)
- 인스턴스: 메모리에 할당된 객체(현실적으로 존재하는 상태)

즉, 모든 인스턴스는 객체지만, 모든 객체가 인스턴스는 아니다.

```java
public static void main(String args[]){
  Book book1; // 객체 O, 인스턴스 X
  Book book2 = new Book(); // 객체 O, 인스턴스 O
}
```

> 그러나 보통은 객체와 인스턴스를 구분하지 않고 같은 의미로 사용한다.
