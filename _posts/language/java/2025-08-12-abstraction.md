---
title: Java - 추상화에 대해 알아보자.
date: 2025-08-12 21:45:00 +0900
categories: [Language, Java]
tags: [java, abstract, oop]
---

## **추상화란?**
추상화란 객체지향 4대 요소 중 하나로, 세부적인 특징은 버리고 핵심 정보들을 추출하여 모델링하는 것을 말한다.

추상화를 하는 이유는 추상화를 함으로써 시스템의 복잡도를 낮출 수 있기 때문이다.

추상화는 아래와 같이 2가지로 나눌 수 있다.
- 데이터 추상화
- 프로세스 추상화

### **데이터 추상화**
예를 들어 설명해보자면 철수, 영희, 돌쇠 이렇게 3명의 사람이 있다고 가정해보자.

이들은 모두 다른 사람들이지만 공통적인 특징들을 가진다.

예시
```java
public abstract class Person {
    private String name;
    private int age;
    private String gender;

    public String getName() { 
      return name; 
    }

    public int getAge() { 
      return age; 
    }

    public String getGender() { 
      return gender; 
    }    
}

public class 철수 extends Person { }

public class 영희 extends Person { }

public class 돌쇠 extends Person { }
```

위 코드처럼 공통적으로 가진 특징들을 추상화하여 분리한 후 사용하면 철수, 영희, 돌쇠 클래스 내부에 복잡도를 떨어뜨릴 수 있다.

정리하자면 데이터 추상화는 어떤 데이터가 중요한지 설계하는 것을 말하는 것이다.

### **프로세스 추상화**
프로세스 추상화란, 내부 구현 로직을 모르더라도 무슨 일을 하는지만 알고 쓸 수 있는 것을 말한다.

이번에도 예시를 들어서 설명해보겠다.

예시
```java
public interface Car {
    int accelerate(int speed);
    int brake(int speed);
}

public class KoreanCar implements Car {
    @Override
    public int accelerate(int speed) {
        return speed + 15;
    }

    @Override
    public int brake(int speed) {
        return speed - 15;
    }
}

public class UsaCar implements Car {
    @Override
    public int accelerate(int speed) {
        return speed + 20;
    }

    @Override
    public int brake(int speed) {
        return speed - 20;
    }
}
```

사용 예시
```java
public class Main{
  public static void main(String[] args){

    Car myCar = new KoreanCar();
    System.out.println(myCar.accelerate(50));

    myCar = new UsaCar();
    System.out.println(myCar.accelerate(50));
  }
}
```

위 예시에서 확인할 수 있듯이 내부 구현 로직을 몰라도 정의된 행위(인터페이스의 메서드)만 보고도 무엇을 하는지 알 수 있게 하는 것이 프로세스 추상화이다.