---
title: "Swift - 일급 함수란?"
date: 2025-09-26 06:50:00 +0900
categories: [Language, Swift]
tags: [swift, function, callback]
---

## **일급 함수란?**
일급 함수란 함수가 일급 객체로 취급되는 것을 의미함.

**일급 함수의 조건**
- 런타임에도 함수 생성 가능
- 매개 변수나 반환값으로 함수 사용 가능
- 변수나 데이터 구조안에 저장 가능
- 함수의 이름과 관계 없이 고유 식별이 가능해야함

### **함수를 변수와 상수 안에 할당 가능**
- 함수를 변수 안에 할당해서 사용이 가능함.
- 이때 변수의 데이터 타입은 일반적으로 사용되는 `String`, `Int`같은 타입이 아닌 함수 타입으로 선언됨.

**함수 타입 형식**
```swift
(인자타입, 인자타입, ...) -> 반환 타입
```

> 반환 타입이 없는 경우 `Void` 또는 `()`를 명시해줘야함.
{: .prompt-tip }

예시
```swift
func test(ex: Int) -> String{
    return "ex: \(ex)"
}


var fn: (Int) -> String = test
print(fn(10))
```

결과
```
ex: 10
```

만약 같은 이름을 가진 함수를 변수에 할당한다면 함수 타입으로 구분해야하며, 함수 타입까지 동일하다면 함수 식별자를 통해 구분해야함.

예시
```swift
func test(ex: Int) -> String{
    return "ex: \(ex)"
}


func test(ex2: Int) -> String{
    return "ex2: \(ex2)"
}

var fn: (Int) -> String = test(ex:)
print(fn(10))

var fn2: (Int) -> String = test(ex2:)
print(fn2(19))
```

결과
```
ex: 10
ex2: 19
```

### **함수의 반환값으로 함수 사용 가능**
- 함수의 반환값으로 함수를 사용 가능함.
- 이때 반환 타입은 함수 타입으로 명시함.

예시
```swift
func printInt(){
    print(20)
}

func returnFn() -> () -> Void{
    return printInt
}

var fn: () -> Void = returnFn()
fn()
```

결과
```
20
```

### **함수의 매개 변수로 함수를 받을 수 있음**
- 함수를 매개 변수로 받아 콜백 함수로 처리가 가능함.

> 콜백 함수란 특정 시점이나 조건이 만족했을 때 실행되도록 넘겨주는 함수를 의미함.
{: .prompt-tip }

예시
```swift
func successHandler(){
    print("함수 처리 성공")
}

func failHandler(){
    print("함수 처리 실패")
}

func test(num: Int, success sFunc: () -> Void, fail fFunc: () -> Void){
    guard num > 0 else{
        fFunc()
        return
    }
    
    print(num)
    sFunc()
}

test(num: 10, success: successHandler, fail: failHandler)
```

결과
```
10
함수 처리 성공
```