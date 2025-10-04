---
title: "Swift - 프로토콜 활용"
date: 2025-10-04 14:31:00 +0900
categories: [Language, Swift]
tags: [swift, protocol, class, struct, enum]
---

## **익스텐션 사용**
- 기존에 정의된 클래스, 구조체, 열거형 등에 익스텐션을 통해 프로토콜을 구현하도록 추가할 수 있음.

구문
```swift
extension 객체명: 프로톹콜명, ....{
  // 구현 내용
}
```

예시
```swift
class TestClass{
    var ex: Int
    
    init(ex: Int) {
        self.ex = ex
    }
}

protocol TestProtocol{
    var description: String { get }
}

extension TestClass: TestProtocol{
    var description: String{
        return "ex : \(self.ex)"
    }
}

var testClass = TestClass(ex: 10)
print(testClass.description)
```

결과
```
ex : 10
```

## **프로토콜 상속**
- 클래스와 마찬가지로 프로토콜끼리 상속이 가능함.
  - 이때 프로토콜은 다중 상속이 지원되기 때문에 여러 프로토콜을 상속받을 수 있음.

구문
```swift
protocol 프로토콜명: 상위프로토콜1, ...{
  // ...
}
```

예시
```swift
protocol TestProtocol{
    var ex: Int { get set }
}

protocol MiddleProtocol{
    var description: String { get }
}

protocol LowProtocol: TestProtocol, MiddleProtocol{
    func plusOne()
}

class TestClass: LowProtocol{
    var ex: Int
    var description: String{
        return "ex : \(self.ex)"
    }
    
    init(ex: Int) {
        self.ex = ex
    }
    
    func plusOne() {
        self.ex += 1
    }
}

var testClass: TestClass = TestClass(ex: 10)
testClass.plusOne()

print(testClass.description)
print(testClass.ex)
```

결과
```
ex : 11
11
```

이때, 상위 프로토콜에 선언되어 있던 내용이 하위 프로토콜과 중복되더라도 상관 없음.

예시
```swift
protocol TestProtocol{
    var ex: Int { get set }
}

protocol MiddleProtocol{
    var description: String { get }
}

protocol LowProtocol: TestProtocol, MiddleProtocol{
    var ex: Int { get set }
    var description: String { get }
    func plusOne()
}

class TestClass: LowProtocol{
    var ex: Int
    var description: String{
        return "ex : \(self.ex)"
    }
    
    init(ex: Int) {
        self.ex = ex
    }
    
    func plusOne() {
        self.ex += 1
    }
}

var testClass: TestClass = TestClass(ex: 10)
testClass.plusOne()

print(testClass.description)
print(testClass.ex)
```

## **클래스 전용 프로토콜**
- 프로토콜은 원래 클래스, 구조체 열거형 등이 모두 구현 가능함.
- 프로토콜을 클래스만 구현 가능하도록 만들 수 있음.

구문
```swift
protocol 프로토콜명: AnyObject{

}
```

> `AnyObject`는 범용 타입으로 모든 클래스를 받을 수 있는 최상위 클래스임.
{: .prompt-tip }

예시
```swift
protocol TestProtocol: AnyObject{
    var ex: Int { get set }
    var description: String { get }
}

class TestClass: TestProtocol{
    var ex: Int
    
    var description: String{
        return "ex : \(ex)"
    }
    
    init(ex: Int) {
        self.ex = ex
    }
}

var testClass = TestClass(ex: 10)
print(testClass.description)
```

결과
```
ex : 10
```

## **optional 키워드**
- 프로토콜에 명시된 프로퍼티, 메서드는 프로토콜을 구현하는 구현체 내에서 반드시 구현하도록 강제됨.
- `optional` 키워드는 이것을 선택적으로 구현하도록 만들어줌.
- `optional`을 사용하기 위해선 `Foundation` 프레임워크에 정의되있는 `@objc` 어노테이션을 같이 사용해줘야함.
  - `@objc`: Objective-C 에서 해당 코드를 참조할 수 있도록 해주는 어노테이션

예시
```swift
import Foundation

@objc
protocol TestProtocol{
    var ex: Int { get set }
    @objc optional var description: String { get }
    
    @objc optional func plusOne()
}

class TestClass: TestProtocol{
    var ex: Int
    
    init(ex: Int) {
        self.ex = ex
    }
    
    func plusOne() {
        self.ex += 1
    }
}

var testClass: TestProtocol = TestClass(ex: 10)
testClass.plusOne?()

if let description = testClass.description{
    // ..
}
```

- `optional` 키워드로 선언된 메서드의 경우엔 `?`를 통해서 안전하게 접근할 수 있음.
  - 반환값이 있을 경우 해당 반환값은 옵셔널 타입으로 감싸져서 반환됨.
- 만약 구현된게 확실한 상황이라면 강제 해제 연산자(`!`)를 통해서 일반 타입으로 반환받는 것도 가능함.
- 프로퍼티의 경우 메서드처럼 `?`를 붙이지 않고 접근할 수 있음.
  - 옵셔널 타입으로 반환되기 때문에 바인딩 필요함.