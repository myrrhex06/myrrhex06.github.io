---
title: "Swift - @escaping과 @autoclosure란?"
date: 2025-09-28 11:22:00 +0900
categories: [Language, Swift]
tags: [swift, function, closure, escaping, autoclosure]
---

## **@escaping이란?**
- 함수의 매개 변수로 받은 클로저를 저장해서 사용할 수 있게 해줌.
  - `@escaping`을 사용하지 않을 경우엔 전달받은 클로저를 변수에 저장할 수 없음.

형식
```swift
func 함수명(매개변수명: @escaping 데이터타입, ..) -> 반환타입{
  실행 구문
}
```

사용 예시
```swift
func test(success: @escaping () -> Void){
    let fn = success
    fn()
}

test{
    print("Hello World!")
}
```

결과
```
Hello World!
```

## **@autoclosure이란?**
- 클로저를 넘기는 것이 아닌 클로저 내부에 들어가는 구문만 받아서 처리할 수 있도록 해줌.
  - 컴파일러가 자동으로 클로저로 감싸주기 때문에 문제가 생기지 않음.
- `{}`로 감싼 클로저를 넘기는 대신 `()`를 통해 구문만 넘김.

예시
```swift
func test(math: @autoclosure () -> Int) -> Int{
    return math()
}

var a = 10
var b = 20

print(test(math: (b - a)))
```

결과
```
10
```