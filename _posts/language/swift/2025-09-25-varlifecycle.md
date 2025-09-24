---
title: "Swift - 변수의 생명 주기"
date: 2025-09-25 06:40:00 +0900
categories: [Language, Swift]
tags: [swift, var, lifecycle]
---

## **변수의 생명 주기란**
- 말 그대로 변수가 메모리에 살아있는 주기를 의미함.
- 변수는 크게 지역(Local) 변수와 전역(Global) 변수로 나뉨.
  - **지역 변수**: 특정 구문 내에서 선언된 변수로, 해당 구문이 끝나면 변수도 사라짐.
  - **전역 변수**: 이름 그대로 전역적으로 선언된 변수로, 일반적으로는 프로그램이 종료되기 전까지 살아있으며 어디서든 접근 가능.
- **스코프**: 변수의 생존 영역을 의미함.

예시
```swift
do{
  // 상위 블록
  var ex1 = 0

  do{
    // 하위 블록
    ex1 = 10
  }

  print(ex1)
}
```

결과
```
10
```

위 예시처럼 상위 블록에서 선언된 변수는 하위 블록에서 접근 가능함.

예시
```swift
do{
  // 상위 블록
  do{
    // 하위 블록
    var ex = 10
    print(ex)
  }

  ex = 20
  print(ex)
}
```

결과
```
error: MyPlayground.playground:7:3: cannot find 'ex' in scope
  ex = 20
  ^~

error: MyPlayground.playground:8:9: cannot find 'ex' in scope
  print(ex)
        ^~
```

반면 위 예시는 오류가 발생하는 것을 확인할 수 있는데, 하위 블록에서 선언된 변수는 하위 블록이 종료되는 시점에서 사라지기 때문에 상위 블록에서는 접근이 불가능함.
