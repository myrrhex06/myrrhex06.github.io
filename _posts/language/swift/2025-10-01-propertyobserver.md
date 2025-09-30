---
title: "Swift - 프로퍼티 옵저버란?"
date: 2025-10-01 06:50:00 +0900
categories: [Language, Swift]
tags: [swift, class, struct, property, didSet, willSet]
---

## **프로퍼티 옵저버란?**
- 저장 프로퍼티의 값이 바뀔 때 전/후처리를 할 수 있도록 해주는 구문을 의미함.
- `willSet`: 값이 바뀌기 전에 호출됨.
  - 새롭게 할당되는 값이 인자값으로 넘어옴.
  - 매개변수명을 생략할 경우 `newValue`라는 상수명으로 접근 가능함.
- `didSet`: 값이 바뀐 후에 호출됨.
  - 기존에 할당되어 있던 값이 인자값으로 넘어옴.
  - 매개변수명을 생략할 경우 `oldValue`라는 상수명으로 접근 가능함.

`willSet` 구문
```swift
var 변수명: 데이터타입 = [초기값]{
  willSet (매개변수명){
    실행 구문
  }
}
```

`didSet` 구문
```swift
var 변수명: 데이터타입 = [초기값]{
  didSet (매개변수명){
    실행 구문
  }
}
```

예시 1
```swift
struct Player{
    var level: Int{
        willSet{
            print("\(level)에서 \(newValue)로 레벨이 변경되었습니다.")
        }
    }
}

var player = Player(level: 10)
player.level = 20
```

결과
```
10에서 20로 레벨이 변경되었습니다.
```

예시 2
```swift
struct BankAccount{
    var balance: Int{
        didSet{
            print("\(oldValue)에서 \(balance)으로 변경되었습니다.")
        }
    }
}

var account = BankAccount(balance: 1000)
account.balance = 2000
```

결과
```
1000에서 2000으로 변경되었습니다.
```

예시 3
```swift
struct Car{
    var speed: Int{
        willSet(value){
            print("\(speed)이 \(value)으로 바뀔 예정")
        }
        didSet{
            print("\(oldValue)이 \(speed)으로 바뀌었다")
        }
    }
}

var car = Car(speed: 10)
car.speed = 20
```

결과
```
10이 20으로 바뀔 예정
10이 20으로 바뀌었다
```

> 프로퍼티 옵저버는 `init` 시에는 호출되지 않음.
{: .prompt-tip }