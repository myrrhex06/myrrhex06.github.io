---
title: "Swift - 옵셔널 체이닝이란?"
date: 2025-10-03 15:43:00 +0900
categories: [Language, Swift]
tags: [swift, optional, class, struct]
---

옵셔널 타입으로 선언된 값을 사용하기 위해선 먼저 옵셔널 언래핑을 해줘야함.<br>
이때 주로 사용하는 방식은 옵셔널 바인딩이지만, 옵셔널 바인딩은 아래와 같은 상황에서 사용하기 번거러움.

예시
```swift
class Test{
    var ex: String?
    
    init(ex: String?){
        self.ex = ex
    }
}

var t: Test? = Test(ex: "Hello World!")

if let t = t{
    if let ex = t.ex{
        print(ex)
    }
}
```

결과
```
Hello World!
```

이렇게 옵셔널을 사용하는 계층이 조금만 깊어지더라도, 중첩 `if`문이 생겨 코드 자체가 지저분해질 수 있음.<br>
이런 문제를 해결하기 위해 옵셔널 체이닝을 사용함.

## **옵셔널 체이닝**
- `?.` 를 통해서 `nil` 값을 안전하게 처리 가능함
- 접근한 값은 옵셔널 타입으로 정의되지 않은 값이라도 반드시 옵셔널 타입으로 반환됨.
- 만약 값이 없다면 `nil`로 반환됨.

옵셔널 체이닝 예시
```swift
class Test{
    var ex: String?
    
    init(ex: String?){
        self.ex = ex
    }
}

var t: Test? = Test(ex: "Hello World!")

if let ex = t?.ex{
    print(ex)
}
```

결과
```
Hello World!
```

위에서 설명했듯이 접근한 값이 옵셔널 타입이 아니더라도, 반드시 옵셔널 타입으로 반환됨.

예시
```swift
class Test{
    var ex1: Int
    var ex2: Int?
    
    init(ex1: Int, ex2: Int? = nil) {
        self.ex1 = ex1
        self.ex2 = ex2
    }
}

class SubTest{
    var test: Test?
    var value: Int
    
    init(test: Test? = nil, value: Int) {
        self.test = test
        self.value = value
    }
}

var test = Test(ex1: 5, ex2: 10)
var subTest: SubTest? = SubTest(test: test, value: 7)


print(subTest?.test?.ex2)
print(subTest?.test?.ex1)
```

결과
```
Optional(10)
Optional(5)
```

마찬가지로 위에서 설명했듯이 만약 값이 `nil`일 경우 `nil` 값이 반환됨.

예시
```swift
class Test{
    var ex1: Int
    var ex2: Int?
    
    init(ex1: Int, ex2: Int? = nil) {
        self.ex1 = ex1
        self.ex2 = ex2
    }
}

class SubTest{
    var test: Test?
    var value: Int
    
    init(test: Test? = nil, value: Int) {
        self.test = test
        self.value = value
    }
    
    func returnTest() -> Test? {
        return self.test
    }
}

var subTest: SubTest? = SubTest(value: 7)
print("ex2: \(subTest?.test?.ex2)")
```

결과
```
ex2: nil
```

> 강제 언래핑(`!`)을 통해 옵셔널 언래핑을 할 경우 위와 같은 상황에서는 에러가 발생하게됨. 하지만, 옵셔널 체이닝을 통해 안전하게 처리가 가능함.
{: .prompt-tip }

옵셔널 값을 반환하는 메서드에도 적용이 가능함.

예시
```swift
class Test{
    var ex1: Int
    var ex2: Int?
    
    init(ex1: Int, ex2: Int? = nil) {
        self.ex1 = ex1
        self.ex2 = ex2
    }
}

class SubTest{
    var test: Test?
    var value: Int
    
    init(test: Test? = nil, value: Int) {
        self.test = test
        self.value = value
    }
    
    func returnTest() -> Test? {
        return self.test
    }
}

var test = Test(ex1: 5, ex2: 10)
var subTest: SubTest? = SubTest(test: test, value: 7)

if let ex2 = subTest?.returnTest()?.ex2{
    print("ex2: \(ex2)")
}
```

결과
```
ex2: 10
```