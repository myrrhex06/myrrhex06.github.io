---
title: 오버로딩과 오버라이딩에 대해 알아보자.
date: 2025-08-11 21:50:00 +0900
categories: [Language, Java]
tags: [java, overloading, oop, overriding]
---

## **오버로딩**
같은 클래스 내의 메서드 이름이 같으나 매개변수의 개수, 타입, 순서를 다르게 해서 같은 이름으로도 여러 개의 메서드를 정의하는 것 뜻한다.

### **오버로딩의 장점**
- 프로그램의 유연성을 높임
- 코드를 깔끔하게 만드는 효과가 있음

### **오버로딩 예시**
```java
public class Main {
    public static void main(String[] args) {
        Example ex = new Example();

        ex.print();
        ex.print(10);
        ex.print(10, "10");
        ex.print("10", 10);
    }
}

class Example{
    void print(){
        System.out.println("Hello World");
    }

    void print(int a){
        System.out.println("Hello World " + a);
    }

    void print(String a, int b){
        System.out.println("Hello World " + a + " " + b);
    }

    void print(int b, String a){
        System.out.println("Hello World " + b + " " + a);
    }
}
```

## **오버라이딩** 
상위 클래스가 가지고 있는 메서드를 하위 클래스가 재정의하는 것을 말한다.

> 상속 관계 클래스에서 사용되며 `static`, `final`로 선언한 메서드는 오버라이딩 불가능

### **오버라이딩 예시**
```java
public class Main {
    public static void main(String[] args) {
        Animal dog = new Dog();

        dog.voice();
    }
}

interface Animal{
    void voice();
}

class Dog implements Animal{
    @Override
    public void voice() {
        System.out.println("멍멍");
    }
}
```

오버로딩과 오버라이딩에 대한 내용은 객체지향 프로그래밍과 밀접한 연관이 되어있으며 JVM 상으로 어떤식으로 구성되는지에 대한 내용들도 연관이 있기 때문에 다룰 내용이 이것보다 매우 많지만, 그 내용들은 추후 다른 글에서 다룰 예정이다.