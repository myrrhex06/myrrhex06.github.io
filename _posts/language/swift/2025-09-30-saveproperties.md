---
title: "Swift - 저장 프로퍼티란?"
date: 2025-09-30 06:50:00 +0900
categories: [Language, Swift]
tags: [swift, class, struct, property]
---

> [이전글](https://myrrhex06.github.io/posts/classstruct1/)과 이어짐

클래스나 구조체에 선언된 변수나 상수를 프로퍼티라고 함.<br>
프로퍼티에는 아래와 같이 두 유형이 있음
- 저장 프로퍼티
- 연산 프로퍼티

## **저장 프로퍼티란?**
- 일반적으로 생각하는 프로퍼티로, 입력된 값을 저장하거나 저장된 값을 제공하는 역할을 함.
- 변수, 상수를 통해 정의 가능함.

예시
```swift
class Person
{
  var name: String // 저장 프로퍼티
  var age: Int // 저장 프로퍼티

  init(name: String, age: Int){
    self.name = name
    self.age = age
  }
}

struct Animal{
  var name: String // 저장 프로퍼티
  var age: Int // 저장 프로퍼티
}
```

### **주의점**
클래스 예시
```swift
class Person
{
  var name: String // 저장 프로퍼티
  var age: Int // 저장 프로퍼티

  init(name: String, age: Int){
    self.name = name
    self.age = age
  }
}

let person = Person(name: "홍길동", age: 20)
person.name = "응애"
print(person.name)
```

결과
```swift
응애
```

위 예시에서는 클래스 인스턴스 변수가 상수(`let`)으로 선언되어 있으나 저장 프로퍼티에 값 수정이 가능함.<br>

클래스 인스턴스 변수는 메모리 주소를 할당받기 떄문에, 저장 프로퍼티를 수정하더라도 할당된 메모리 주소는 바뀌지 않음.<br>
이런 이유 때문에 수정이 가능함.

구조체 예시
```swift
struct Animal{
  var name: String // 저장 프로퍼티
  var age: Int // 저장 프로퍼티
}

let animal = Animal(name: "뽀삐", age: 3)
animal.name = "뽀삐삐"
print(animal.name)
```

결과
```
error: MyPlayground.playground:7:8: cannot assign to property: 'animal' is a 'let' constant
animal.name = "뽀삐삐"
```

위 구조체 예시에서는 상수(`let`)로 선언된 구조체 인스턴스를 수정하려 할 경우 오류가 발생함.

구조체는 값타입이기 때문에 메모리 주소가 아닌 구조체 자체가 복사되어 들어감. 이로 인해 저장 프로퍼티 값을 수정할 경우 할당된 구조체 자체가 변하기 때문에 오류가 발생하는 것임.

## **지연 저장 프로퍼티**
- `lazy` 키워드를 통해 저장 프로퍼티의 초기화를 지연시킬 때 사용함.
  - 프로퍼티를 바로 초기화 하지 않고 프로퍼티에 접근 시 초기화됨.

> 이때, `lazy` 키워드는 오직 `var`로 선언된 변수에만 사용 가능함. 왜냐하면 `let`은 선언과 동시에 초기화가 되어야하고, `lazy`는 초기화를 지연시키기 때문에 서로 맞지 않음.

예시
```swift
class LazyTest{
    init(){
        print("Lazy 초기화 테스트")
    }
}

class Test{
    lazy var lazyTest = LazyTest()
    
    init(){
        print("Test 인스턴스 생성")
    }
}

var test = Test()
```

결과
```
Test 인스턴스 생성
```

위 예시를 보면 아직 `lazyTest`가 초기화되지 않은 것을 확인할 수 있음.

예시
```swift
class LazyTest{
    init(){
        print("Lazy 초기화 테스트")
    }
}

class Test{
    lazy var lazyTest = LazyTest()
    
    init(){
        print("Test 인스턴스 생성")
    }
}

var test = Test()
test.lazyTest
```

결과
```
Test 인스턴스 생성
Lazy 초기화 테스트
```

`lazyTest`에 접근할 때 초기화되는 것을 확인할 수 있음.

### **클로저를 사용한 초기화**
- 저장 프로퍼티를 클로저를 통해 초기화 가능함.
- 연산이나 로직 처리를 통한 값을 이용해서 초기화할 때 사용함.
- 정의된 클로저 구문은 인스턴스 생성 시 한번 실행된 후 재실행되지 않음.

> 연산 프로퍼티와 유사하지만, 연산 프로퍼티는 참조될 때마다 매번 재실행한 후 반환함.

구문
```swift
var/let 프로퍼티명: 타입 = {
  실행 구문
  return 반환값
}()
```

예시
```swift
class Test{
    var desc: String = {
        print("초기화됨")
        return "Hello World!"
    }()
}

var test: Test = Test()
print(test.desc)
```

결과
```
초기화됨
Hello World!
```

앞서 설명한 `lazy`와 함께 사용도 가능함.

구문
```swift
lazy var 프로퍼티명: 타입 = {
	실행 구문
	return 반환값
}()
```

예시
```swift
class Test{
    lazy var desc: String = {
        print("초기화됨")
        return "Hello World!"
    }()
}

var test: Test = Test()
```

결과는 아직 아무것도 나오지 않음.

예시
```swift
class Test{
    lazy var desc: String = {
        print("초기화됨")
        return "Hello World!"
    }()
}

var test: Test = Test()
print(test.desc)
```

결과
```
초기화됨
Hello World!
```

프로퍼티에 접근 시 초기화되는 것을 확인할 수 있음.