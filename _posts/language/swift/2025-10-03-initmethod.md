---
title: "Swift - 초기화 메서드(생성자)란?"
date: 2025-10-03 14:25:00 +0900
categories: [Language, Swift]
tags: [swift, class, struct, init]
---

## **초기화 메서드(생성자)란?**
- 클래스, 구조체를 초기화할 땐 반드시 저장 프로퍼티에 초기값이 할당되어야함.
- 초기화 메서드를 통해 클래스, 구조체에 정의된 저장 프로퍼티에 값을 할당할 수 있음.
- 이때, 초기화 메서드명은 반드시 `init`으로 작성해야만함.
  - `init` 이외에 메서드명은 컴파일러에서 인식하지 못함.

구문
```swift
init(매개변수명: 데이터타입, ..){
  실행구문
}
```

예시
```swift
class TestClass{
    
    var ex1: Int
    var ex2: Int
    
    init(ex1: Int, ex2: Int){
        self.ex1 = ex1
        self.ex2 = ex2
        print("초기화 구문 실행 종료")
    }
}

struct TestStruct{
    
    var ex1: Int
    var ex2: Int
    
    init(ex1: Int, ex2: Int) {
        self.ex1 = ex1
        self.ex2 = ex2
    }
}

var testClass = TestClass(ex1: 10, ex2: 20)
print(testClass.ex1)
print(testClass.ex2)

var testStruct = TestStruct(ex1: 10, ex2: 20)
print(testStruct.ex1)
print(testStruct.ex2)
```

결과
```
초기화 구문 실행 종료
10
20
10
20
```

> `init`을 직접 정의할 경우 기본적으로 제공되는 기본 초기화 메서드는 더는 사용할 수 없게 되며, 구조체에서 기본적으로 제공하는 멤버와이즈 `init` 또한 제공되지 않음.
{: .prompt-tip }

초기화 메서드를 오버로딩해서 여러 개를 만드는 것도 가능함.

예시
```swift
class TestClass{
    
    var ex1: Int
    var ex2: Int
    
    init(){
        self.ex1 = 10
        self.ex2 = 20
    }
    
    init(ex1: Int){
        self.ex1 = ex1
        self.ex2 = 20
    }
    
    init(ex1: Int, ex2: Int){
        self.ex1 = ex1
        self.ex2 = ex2
        print("초기화 구문 실행 종료")
    }
}

struct TestStruct{
    
    var ex1: Int
    var ex2: Int
    
    init(){
        self.ex1 = 10
        self.ex2 = 20
    }
    
    init(ex1: Int){
        self.ex1 = ex1
        self.ex2 = 20
    }
    
    init(ex1: Int, ex2: Int) {
        self.ex1 = ex1
        self.ex2 = ex2
    }
}

var testClass1 = TestClass()
var testClass2 = TestClass(ex1: 10)
var testClass3 = TestClass(ex1: 10, ex2: 20)

var testStruct1 = TestStruct()
var testStruct2 = TestStruct(ex1: 10)
var testStruct3 = TestStruct(ex1: 10, ex2: 20)
```

## **초기화 메서드(생성자) 오버라이딩**
- 클래스에서 선언한 초기화 메서드도 메서드의 일종으로 하위 클래스에서 오버라이딩이 가능함.
- 하위 클래스에서 초기화 메서드를 오버라이딩할 경우 상위 클래스에서 정의한 초기화 메서드가 사용되지 않기 때문에 `super`를 통해 호출해줘야함.

예시 1
```swift
class TestClass{
    init(){
        print("TestClass Init")
    }
}

class SubTestClass: TestClass{
    override init(){
        // super.init()
    }
}

var sub: SubTestClass = SubTestClass()
```

결과
```
TestClass Init
```

- 하위 클래스인 `SubTestClass`가 초기화 메서드를 정의할 경우 상위 클래스인 `TestClass`에서는 이미 정의되어 있기 때문에 오버라이딩처리를 해줘야함.
- 상위 클래스 내부에 생성자가 기본 생성자 1개만 존재한다면, 명시적으로 `super.init()`을 처리하지 않더라도 컴파일러가 자동으로 호출해주기 때문에 적어주지 않아도 됨.

예시 2
```swift
class TestClass{
    var ex1: Int
    
    init(){
        self.ex1 = 10
        print("Init")
    }
    
    init (ex1: Int){
        self.ex1 = ex1
        print("ex1 Init")
    }
}

class SubTestClass: TestClass{
    
    var ex: Int
    
    override init(){
        self.ex = 10
        super.init()
    }
    
    init(ex: Int, ex1: Int){
        self.ex = ex
        super.init(ex1: ex1)
    }
}

var sub: SubTestClass = SubTestClass(ex: 10, ex1: 20)
print(sub.ex)
print(sub.ex1)
```

결과
```
ex1 Init
10
20
```

- 여러 개의 초기화 메서드가 오버로딩되어 있을 경우엔 명시적으로 호출할 초기화 메서드를 선언해줘야함.