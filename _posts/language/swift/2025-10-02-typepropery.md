---
title: "Swift - 타입 프로퍼티란?"
date: 2025-10-02 06:45:00 +0900
categories: [Language, Swift]
tags: [swift, class, struct, property, static]
---

## **타입 프로퍼티란?**
- 객체 인스턴스를 생성하지 않고 클래스나 구조체에 직접 접근하여 사용할 수 있는 프로퍼티를 의미함.
- 타입 프로퍼티는 애플리케이션 내에서 공유 자원으로 사용됨.
  - 동시성 문제가 생길 수 있음.
- `static`, `class` 키워드를 통해 선언 가능함.
  - `static`: 구조체, 클래스 모두 사용 가능함.
  - `class`: 클래스에서만 사용 가능하며 저장 프로퍼티에는 사용할 수 없고, 연산 프로퍼티에만 사용 가능함.
    - `class` 키워드를 통해 타입 프로퍼티를 선언할 경우 해당 클래스를 상속 받은 하위 클래스에서 재정의가 가능함.

`static` 구문
```swift
static var 또는 let 프로퍼티명: 데이터타입 = 초기값
```

> `static`으로 선언한 저장 프로퍼티의 경우 `init`을 통한 초기값 지정이 불가능하기 때문에 초기값이 반드시 존재해야함.
{: .prompt-tip }

`class` 구문
```swift
class var 프로퍼티명: 데이터타입{
  get{

  }
  set{

  }
}
```

`static` 구문 예시
```swift
struct Test{
    static var name: String = "홍길동"
    
    static var description: String{
        get{
            return "이름은: \(name)"
        }
    }
}

print(Test.name)
print(Test.description)
```

결과
```
홍길동
이름은: 홍길동
```

`class` 구문 예시
```swift
class Test{
    class var test: String{
        return "Hello World!"
    }
}

print(Test.test)
```

결과
```
Hello World!
```