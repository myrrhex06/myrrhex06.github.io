---
title: "Swift - 프로토콜이란?"
date: 2025-10-04 11:20:00 +0900
categories: [Language, Swift]
tags: [swift, protocol, class, struct, enum]
---

## **프로토콜이란?**
- 객체의 설계도로 프로퍼티나 메서드를 구현하는 것이 아닌 단순 역할만 정의해둔 것.
  - Java의 인터페이스와 유사함.
- 프로토콜을 구현하는 클래스나 구조체, 열거형은 프로토콜의 명시되어 있는 프로퍼티와 메서드를 반드시 구현해야만함.

구문
```swift
protocol 프로토콜명{
  // ..
}
```

예시
```swift
protocol TestProtocol{
    var ex1: Int { get set }
    var description: String { get }
    
    func plusOne() -> Void
}
```

프로퍼티를 명시할 때는 위 예시처럼 `get`, `set` 구문을 넣어 선언 가능하며, 저장 프로퍼티를 명시하고 싶을 경우 `get`, `set` 두 구문 모두 명시해줘야함.

> 프로토콜은 선언된 프로퍼티가 저장 프로퍼티인지, 연산 프로퍼티인지 구분하지 않고 단순히 `get`, `set`만을 명시함.
{: .prompt-tip }

프로토콜 구현 구문
```swift
class 또는 struct 또는 enum: 프로토콜명{
  // 구현 코드
}
```

예시
```swift
protocol TestProtocol{
    var ex1: Int { get set }
    var description: String { get }
    
    func plusOne()
}

class TestClass: TestProtocol{
    var ex1: Int
    
    var description: String{
        return "ex1 : \(self.ex1)"
    }
    
    init(ex1: Int) {
        self.ex1 = ex1
    }
    
    func plusOne() {
        self.ex1 += 1
    }
}

var testClass = TestClass(ex1: 10)
testClass.plusOne()
print(testClass.description)
print(testClass.ex1)
```

결과
```
ex1 : 11
11
```

구조체, 열거형의 경우 값 타입이기 때문에 만약 메서드 내에서 프로퍼티에 접근하기 위해선 `mutating` 키워드를 명시해줘야함.

예시
```swift
protocol TestProtocol{
    var ex1: Int { get set }
    var description: String { get }
    
    mutating func plusOne()
}

struct TestStruct: TestProtocol{
    var ex1: Int
    
    var description: String{
        return "ex1 : \(self.ex1)"
    }
    
    mutating func plusOne(){
        self.ex1 += 1
    }
}

var testStruct = TestStruct(ex1: 10)
testStruct.plusOne()
print(testStruct.description)
print(testStruct.ex1)

class TestClass: TestProtocol{
    var ex1: Int
    
    init(ex1: Int) {
        self.ex1 = ex1
    }
    
    var description: String{
        return "ex1 : \(self.ex1)"
    }
    
    func plusOne() {
        ex1 += 1
    }
}

var testClass = TestClass(ex1: 10)
testClass.plusOne()
print(testClass.description)
print(testClass.ex1)
```

결과
```
ex1 : 11
11
ex1 : 11
11
```

> 클래스의 경우 `mutating` 키워드를 붙이지 않기 때문에 생략해야함. 만약 붙일 경우 오류가 발생하게됨.
{: .prompt-tip }

### **타입 프로퍼티와 타입 메서드**
- 타입 프로퍼티, 타입 메서드도 명시 가능함.
  - 단, `class` 키워드를 통해 타입 프로퍼티를 명시할 수 없으며 오직 `static` 키워드만으로 명시 가능함.

예시
```swift
protocol TestProtocol{
    static var ex1: Int { get set }
    static func description()
}

struct TestStruct: TestProtocol{
    static var ex1: Int = 10
    
    static func description(){
        print("ex1 : \(self.ex1)")
    }
}

print(TestStruct.ex1)
TestStruct.description()

class TestClass: TestProtocol{
    static var ex1: Int = 10
    
    class func description() {
        print("ex1 : \(self.ex1)")
    }
}

print(TestClass.ex1)
TestClass.description()
```

결과
```
10
ex1 : 10
10
ex1 : 10
```

클래스에서 해당 프로토콜을 구현할 때 내부 구현에서 `static` 키워드를 `class` 키워드로 바꿔서 구현 가능함.

### **초기화 메서드**
- 초기화 메서드를 선언할 수 있음.
- 클래스가 만약 해당 프로토콜을 구현할 경우 초기화 구문을 구현할 때 반드시 `required` 키워드를 붙여줘야함.
  - 구조체는 안붙여도 됨.
- 구조체의 경우 초기화 구문이 정의되어 있지 않다면 멤버와이즈 초기화 구문이 제공되지만, 프로토콜에 명시적으로 초기화 구문이 선언되어 있을 경우 멤버와이즈 초기화 구문이 제공되지 않기 때문에 직접 구현해야함.

예시
```swift
protocol TestProtocol{
    init()
    init(ex1: Int)
}

class TestClass: TestProtocol{
    var ex1: Int
    
    required init(){
        self.ex1 = 10
    }
    
    required init(ex1: Int) {
        self.ex1 = ex1
    }
}

struct TestStruct: TestProtocol{
    var ex1: Int
    
    init() {
        self.ex1 = 10
    }
    
    init(ex1: Int) {
        self.ex1 = ex1
    }
}
```

만약, 클래스가 어떠한 클래스의 하위 클래스로 상위 클래스에서 프로토콜에 정의된 초기화 구문과 동일한 구조로 정의가 되어 있을 경우 `override`, `required` 구문을 모두 명시해줘야함.

예시
```swift
protocol TestProtocol{
    init()
    init(ex1: Int)
}

class TestClass{
    var ex1: Int
    
    init(){
        self.ex1 = 10
    }
    
    init(ex1: Int){
        self.ex1 = ex1
    }
}

class SubTestClass: TestClass, TestProtocol{
    override required init(){
        super.init()
    }
    
    override required init(ex1: Int) {
        super.init(ex1: ex1)
    }
}

var sub: SubTestClass = SubTestClass(ex1: 10)
print(sub.ex1)
```

결과
```
10
```

### **프로토콜 타입 활용**
- Java의 인터페이스처럼 타입으로 프로토콜을 명시한 후 해당 프로토콜을 구현한 구현체의 인스턴스를 할당하여 사용하는 것도 가능함.

예시
```swift
protocol TestProtocol{
    var ex1: Int { get set }
    
    func description() -> String
}

class TestClass: TestProtocol{
    var ex1: Int
    
    init(ex1: Int) {
        self.ex1 = ex1
    }
    
    func description() -> String {
        return "ex1 : \(self.ex1)"
    }

    func test() -> String{
        return "Hello World!"
    }
}

var testProtocol: TestProtocol = TestClass(ex1: 10)
```

단, 이렇게 구현체를 할당할 경우 할당된 값은 구현체이지만 변수는 프로토콜 타입으로 선언되어 있기 때문에 프로토콜에 명시되어 있는 프로퍼티, 메서드에만 접근 가능함.

프로토콜 여러 개를 타입으로 설정하여 해당 프로토콜을 모두 구현한 인스턴스만 받도록 하는 것도 가능함.

예시
```swift
protocol TestProtocolOne{
    func testOne()
}

protocol TestProtocolTwo{
    func testTwo()
}

class TestClass: TestProtocolOne, TestProtocolTwo{
    func testOne() {
        
    }
        
    func testTwo() {
        
    }
    
    func description() {
            
    }
}

let testProtocol: TestProtocolOne & TestProtocolTwo = TestClass()
testProtocol.testOne()
testProtocol.testTwo()
```

마찬가지로 프로토콜에 명시되어 있는 프로퍼티, 메서드에만 접근 가능함.