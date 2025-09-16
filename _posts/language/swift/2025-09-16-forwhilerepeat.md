---
title: "Swift - 반복문이란?"
date: 2025-09-16 20:20:00 +0900
categories: [Language, Swift]
tags: [swift, for, while, repeat-while, loop]
---

## **반복문이란?**
특정 코드를 반복해서 실행해주는 구문을 뜻함.

Swift는 아래 세가지 구문을 제공해줌
- `for`
- `while`
- `repeat-while`

### **for**
정해진 횟수만큼 반복할 때 사용

형식
```swift
for row in range{
  // 반복 코드
}
```

예시
```swift
for row in 1...5{
  print(row)
}
```

결과
```
1
2
3
4
5
```

### **while**
조건식이 참(true)일 동안 실행됨

형식
```swift
while conditional{
  // 반복코드
}
```

예시
```swift
var n: Int = 1
while n <= 5{
    print(n)
    n += 1
}
```

결과
```
1
2
3
4
5
```

### **repeat-while**
`while`문과 유사한 반복문으로, `while`은 조건식을 먼저 검사한 후 반복을 실행하지만 `repeat`은 무조건 한번은 실행된 후 조건문을 검사함

형식
```swift
repeat{
  // 반복 코드
}while conditional
```

예시
```swift
var n: Int = 10
repeat{
  print(n)
}while n < 10
```

결과
```
10
```

위 예시처럼 조건식이 거짓임에도 한번 실행된 것을 확인할 수 있음