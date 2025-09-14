---
title: "Swift - 타입 어노테이션과 타입 추론이란?"
date: 2025-09-15 06:30:00 +0900
categories: [Language, Swift]
tags: [swift, type, typeannotation, let, var]
---

## **타입 어노테이션**
변수나 상수를 선언할 때 명시적으로 타입을 선언해주는 것을 말함

형식
```swift
let letName: dataType
var varName: dataType
```

예시
```swift
var example: String
```

타입 어노테이션을 붙이지 않고, 바로 초기화를 할 경우 타입 추론기가 들어온 값에 맞춰 타입을 추론해줌

```swift
var example = "Hello World!" // String 타입으로 추론됨
```

만약 변수의 선언과 초기화를 분리할 경우엔 변수 선언 시에 타입 어노테이션을 사용해야만함

예시
```swift
// X
var exampleVar
exampleVar = "Hello World"

// O
var example: String
example = "Hello World"
```