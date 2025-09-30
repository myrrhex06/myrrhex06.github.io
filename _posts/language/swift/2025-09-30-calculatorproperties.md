---
title: "Swift - 연산 프로퍼티란?"
date: 2025-09-30 18:30:00 +0900
categories: [Language, Swift]
tags: [swift, class, struct, property]
---

> [이전글](https://myrrhex06.github.io/posts/saveproperties/)과 이어짐.

## **연산 프로퍼티란?**
- 값을 저장하는 것이 아니라 다른 프로퍼티의 값을 연산 후 제공해줌.
- 연산 프로퍼티 값에 접근하기 위해 내부적으로 `get` 구문을 정의해줘야함.
  - 이때 `get` 구문은 값에 접근하기 위해서는 반드시 필요하기 때문에 정의해주지 않으면 값에 접근할 수 없음.
- 연산 프로퍼티에 값을 전달 할 때는 `set` 구문을 정의해 처리 가능함.
  - `set` 구문은 필수적인 요소는 아니라 필요에 따라 사용하면 됨.
  - `set` 구문의 매개변수명을 생략할 경우 기본으로 제공되는 상수명인 `newValue`에 넘어온 값이 담기게 됨.
  - `set` 구문이 정의되어 있지 않은 연산 프로퍼티는 읽기 전용 프로퍼티의 역할만을 수행함.

**구문**
```swift
class/struct 객체명{
  var 프로퍼티명: 데이터타입{
    get{
      처리구문
    }
    set(매개변수명){
      처리구문
    }
  }
}
```

> 연산 프로퍼티의 경우 저장 프로퍼티와는 달리 저장소가 존재하지 않는 계산 전용 프로퍼티기 때문에 `let`으로는 선언 불가능함.
{: .prompt-tip }

예시
```swift
struct TV{
    var width: Int
    var height: Int
    
    var area: Int {
        get{
            print("area get 구문 실행")
            return width * height
        }
        set(newArea){
            width = newArea / height
        }
    }
    
    var perimeter: Int{
        print("perimeter get 구문 실행")
        return 2 * (width + height)
    }
    
    var description: String{
        print("description get 구문 실행")
        return "가로: \(width), 세로: \(height), 넓이: \(area), 테두리: \(perimeter) "
    }
}

var tv = TV(width: 10, height: 20)
print(tv.area)
print(tv.perimeter)
print(tv.description)

tv.area = 20
print(tv.area)
print(tv.perimeter)
print(tv.description)
```

결과
```
area get 구문 실행
200
perimeter get 구문 실행
60
description get 구문 실행
area get 구문 실행
perimeter get 구문 실행
가로: 10, 세로: 20, 넓이: 200, 테두리: 60 
area get 구문 실행
20
perimeter get 구문 실행
42
description get 구문 실행
area get 구문 실행
perimeter get 구문 실행
가로: 1, 세로: 20, 넓이: 20, 테두리: 42 
```

연산 프로퍼티 내에서 `set` 구문을 사용하지 않고, `get` 구문만 사용할 경우 위 예시처럼 구문을 생략하여 사용이 가능함.

> 연산 프로퍼티는 저장 프로퍼티와 달리 `lazy` 키워드를 붙일 수 없는데, 연산 프로퍼티는 저장 공간이 없는 계산 용도이고, `lazy` 키워드는 실제 메모리 공간이 있어야 사용 가능하기 때문에 `lazy` 키워드를 붙일 수 없음.
{: .prompt-tip }