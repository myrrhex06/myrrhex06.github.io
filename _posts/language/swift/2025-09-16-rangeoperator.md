---
title: "Swift - 범위 연산자란?"
date: 2025-09-16 06:40:00 +0900
categories: [Language, Swift]
tags: [swift, operator, range]
---

## **범위 연산자란?**
Swift에서 제공하는 고유 문법으로 범위 내에 해당하는 값들을 표현할 때 사용

범위 연산자는 아래 두 유형이 존재함
- 닫힘 범위 연산자
- 반닫힘 범위 연산자

### **닫힘 범위 연산자**
형식
```swift
a...b
```

- a ~ b까지 포함하는 값들을 뜻함

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

### **반닫힘 범위 연산자**
형식
```swift
a..<b
```

- a는 포함하지만 b는 포함하지 않는 값들을 표현

예시
```swift
for row in 1..<5{
  print(row)
}
```

결과
```
1
2
3
4
```