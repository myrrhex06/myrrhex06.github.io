---
title: "Swift - 예외 처리란?"
date: 2025-10-04 17:33:00 +0900
categories: [Language, Swift]
tags: [swift, error, try, catch, do, enum]
---

## **예외 처리란?**
런타임 중 발생할 수 있는 예외를 처리하지 않으면 애플리케이션이 다운되는 상황이 발생할 수 있음.<br>

이런 상황을 막기 위해 발생하거나 특정 조건에 맞지 않아 예외를 던져 고의적으로 발생시킨 예외를 처리할 수 있음.

### **사용자 정의 예외**
- 직접 사용할 예외를 정의할 수 있음.
- 주로 열거형(`enum`)으로 정의하며, 예외로 사용될 열거형은 `Error` 프로토콜을 반드시 구현해야함.
  - `Error` 프로토콜을 구현하면 컴파일러에서 해당 열거형이 예외를 정의한 열거형임을 알 수 있음.

예시
```swift
enum CustomError: Error{
    case overflowInt
    case underflowInt
}
```

### **예외 던지기**
- 정의한 예외를 함수에서 던지기 위해선 함수 선언문에 `throws` 키워드를 명시해줘야함.
- 예외를 던질 땐 `throw` 키워드를 사용해서 던짐.

> `throw` 키워드는 `throws` 키워드로 예외가 발생할 수 있는 함수인게 명시된 상태에서만 사용 가능함.
{: .prompt-tip }

예시
```swift
enum CustomError: Error{
    case overflowInt
    case underflowInt
}

func test(num: Int) throws{
    guard num > 0 else{
        throw CustomError.underflowInt
    }
    
    guard num < 100 else{
        throw CustomError.overflowInt
    }
    
    print("num : \(num)")
}
```

클로저의 경우엔 아래와 같은 형태로 명시 가능함.

예시
```swift
{(매개변수: 타입, ....) throws -> 반환타입 in
  ...
}
```

`throws` 키워드가 명시된 함수를 사용하기 위해선 `try` 키워드를 반드시 같이 사용해야함.

예시
```swift
enum CustomError: Error{
    case overflowInt
    case underflowInt
}

func test(num: Int) throws{
    guard num > 0 else{
        throw CustomError.underflowInt
    }
    
    guard num < 100 else{
        throw CustomError.overflowInt
    }
    
    print("num : \(num)")
}

try test(num: 10)
```

결과
```
num : 10
```

> 만약 `try` 키워드를 붙이지 않을 경우 컴파일 에러가 발생하게됨.
{: .prompt-tip }

### **예외 캐치**
- `try` 키워드는 단순히 `throws` 키워드가 붙은 함수를 실행한다는 의미임.
  - 예외가 발생하는 경우 처리할 수 없음.
- `do-catch` 문을 통해 예외가 발생하는 경우 처리가 가능함.

구문
```swift
do{
  // 예외가 발생할 수 있는 코드
}catch 예외타입1{
  // 처리 구문
}catch 예외타입2{
  // 처리 구문
}catch{
  // 처리 구문
}
```

예시
```swift
enum CustomError: Error{
    case overflowInt
    case underflowInt
}

func test(num: Int) throws{
    guard num > 0 else{
        throw CustomError.underflowInt
    }
    
    guard num < 100 else{
        throw CustomError.overflowInt
    }
    
    print("num : \(num)")
}

do{
    try test(num: 101)
}catch CustomError.overflowInt{
    print("큰 범위의 숫자")
}catch CustomError.underflowInt{
    print("너무 작은 숫자")
}catch{
    print("알 수 없는 예외 터짐 : \(error)")
}
```

결과
```
큰 범위의 숫자
```

> 예외 타입이 명시되지 않은 `catch` 문에서는 발생한 예외를 `error`라는 이름으로 접근 가능함.
{: .prompt-tip }

### **try?**
- `do-catch`문을 사용하지 않고 반환값을 옵셔널로 받을 수 있게 함.
  - 예외 발생 시 `nil` 값이 반환됨.
- 예외가 발생하더라도 프로그램이 죽지 않기 때문에 주로 예외가 발생했을 무시하고 싶은 경우에 사용함.

예시
```swift
enum CustomError: Error{
    case overflowInt
    case underflowInt
}

func test(num: Int) throws -> Int{
    guard num > 0 else{
        throw CustomError.underflowInt
    }
    
    guard num < 100 else{
        throw CustomError.overflowInt
    }
    
    print("num : \(num)")
    return num;
}

let result = try? test(num: -1)
print("result : \(result)")
```

결과
```
nil
```

### **try!**
- `throws` 키워드로 예외가 발생할 수 있는 함수이지만, 절대 예외가 발생하지 않는다고 확신할 때 사용할 수 있음.
  - 예외가 발생할 경우 `do-catch` 문으로 감싸도 `catch`문에 잡히지 않고 바로 프로그램이 죽게됨.

예시
```swift
enum CustomError: Error{
    case overflowInt
    case underflowInt
}

func test(num: Int) throws{
    guard num > 0 else{
        throw CustomError.underflowInt
    }
    
    guard num < 100 else{
        throw CustomError.overflowInt
    }
    
    print("num : \(num)")
}

try! test(num: 10)
```

결과
```
num : 10
```