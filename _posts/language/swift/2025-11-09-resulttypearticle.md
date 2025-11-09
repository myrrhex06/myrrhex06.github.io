---
title: "Swift - Result 타입이란?"
date: 2025-11-09 09:57:00 +0900
categories: [Language, Swift]
tags: [swift, result, generic, success, fail, try, error, catch]
---

기존 `Error`를 던지는 코드들에는 `throws`와 `do-catch` 문을 통해 예외를 처리해줘야하기 때문에 번거로운 경우가 많았지만, `Result` 타입을 통해 이 문제를 해결 가능함.

**Apple Developer Document**

![image](/assets/img/resulttypedocument.png)

열거형으로 선언되어 `success`, `failure` 두 가지 값을 가지고 있으며, 이 값을 통해 편리하게 성공, 실패를 처리할 수 있음.

사용 예시
```swift
import UIKit

// 사용자 정의 예외
enum CustomError: Error{
    case exError1
    case exError2
}

// Result 타입 반환
func customErrorThrowExample(value: Int) -> Result<Bool, CustomError>{
    if value > 100 {
        return Result.failure(CustomError.exError1) // 실패 반환
    }
    else if value < 0 {
        return Result.failure(CustomError.exError2) // 실패 반환
    }else{
        return Result.success(true) // 성공 반환
    }
}

// 함수 호출
let result = customErrorThrowExample(value: 101)

// Result 타입 분기 처리
switch result {
    // 성공 시 처리
case .success(let result):
    print(result)
    // 실패 시 처리
case .failure(let error):
    print(error)
}
```

결과
```
exError1
```

> `Result` 타입을 사용하면 비동기 콜백이나 네트워크 응답 처리 시 `try-catch` 없이 명확한 성공, 실패 흐름을 쉽게 처리할 수 있음.
{: .prompt-tip }