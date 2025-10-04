---
title: "Swift - 익스텐션(Extension)이란?"
date: 2025-10-04 10:13:00 +0900
categories: [Language, Swift]
tags: [swift, class, struct, extension, method, property, enum]
---

## **익스텐션(Extension)이란?**
- 기존에 이미 정의되어 있는 클래스, 구조체, 열거형 등에 새로운 기능을 추가할 수 있게 해줌.
  - 단, 기존에 정의되어 있는 프로퍼티나 메서드를 오버라이딩하는 것은 불가능함.

구문
```swift
extension 객체명{
	// 추가기능
}
```

### **연산 프로퍼티 익스텐션**
- 익스텐션을 통해 추가할 수 있는 프로퍼티는 연산 프로퍼티임.
  - 저장 프로퍼티는 추가할 수 없음.
  - 연산 프로퍼티이기만 하면 타입 프로퍼티던 인스턴스 프로퍼티던 상관 없음.

예시
```swift
class Test{
    var num1: Int
    
    init(num1: Int) {
        self.num1 = num1
    }
}

extension Test{
    var description: String{
        return "num1 : \(self.num1)"
    }
}

var test: Test = Test(num1: 10)
print(test.description)
```

결과
```
num1 : 10
```

### **메서드 익스텐션**
- 기존에 정의되어 있던 메서드를 오버로딩해서 추가 정의하거나, 새로운 메서드를 추가할 수 있음.
  - 위에서 설명했듯이 오버라이딩은 불가능함.

예시
```swift
class TestClass{
    var num: Int
    
    init(num: Int) {
        self.num = num
    }
}


extension TestClass{
    func plusOne() -> Void{
        self.num += 1
    }
}

var testClass = TestClass(num: 10)
testClass.plusOne()
print(testClass.num)

struct TestStruct{
    var num: Int
}

extension TestStruct{
    mutating func plusOne() -> Void{
        self.num += 1
    }
}

var testStruct: TestStruct = TestStruct(num: 10)
testStruct.plusOne()
print(testStruct.num)
```

결과
```
11
11
```

> 구조체, 열거형의 경우 값타입이기 때문에 `self`를 통해 접근하기 위해선 `mutating` 키워드를 붙여줘야함.