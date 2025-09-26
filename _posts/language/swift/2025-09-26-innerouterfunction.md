---
title: "Swift - 함수의 중첩"
date: 2025-09-26 21:50:00 +0900
categories: [Language, Swift]
tags: [swift, function, callback]
---

## **함수의 중첩**
- 함수 안에 또 다른 함수를 정의할 수 있음.
  - 이때 바깥쪽에 있는 함수를 외부 함수, 안쪽에 있는 함수를 내부 함수라고 함.

예시
```swift
func outFunc(){
    print("외부 함수")
    func inFunc(){
        print("내부함수")
    }
    
    inFunc()
}

outFunc()
```

결과
```
외부 함수
내부함수
```

위 예시처럼 선언된 내부 함수는 함수 밖에서는 접근이 불가능함.<br>
하지만, 내부 함수를 반환함으로써 내부 함수에 영구적으로 접근 가능하도록 할 수 있음.

예시
```swift
func outFunc() -> () -> Void{
    func inFunc(){
        print("반환됨.")
    }
    
    return inFunc
}

let fn = outFunc()
fn()
```

결과
```
반환됨.
```

만약 내부 함수에서 외부 함수에 선언된 지역 변수를 접근할 경우 이 내부 함수를 반환하게 되면 외부 함수에 선언된 지역 변수 값이 캡처됨.

예시
```swift
func outFunc(num: Int) -> () -> Int{
    var value = num
    
    func inFunc() -> Int{
        return value + 100
    }
    
    return inFunc
}

let fn = outFunc(num: 10)

print(fn())
```

결과
```
110
```

이런 처리가 가능한 이유는 내부 함수가 반환될 때 클로저를 갖기 때문임.

클로저란 내부 함수와 그 내부 함수가 실행될 때 필요한 외부 변수들의 값을 묶어둔 객체임.

쉽게 생각하면 클로저는 내부 함수에서 참조하는 모든 외부 변수나 상수의 값, 내부 함수에서 참조하는 다른 객체까지 모두 들고 다니는 바구니라고 생각하면 됨.

> 클로저 내부에 캡처된 값은 외부 함수가 종료되더라도 유지되는 특징을 가짐.
{: prompt-tip }

외부 함수에서 내부 함수를 반환할 때 이 내부 함수는 클로저 형태로 반환됨.