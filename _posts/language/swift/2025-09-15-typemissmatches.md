---
title: "Swift - 서로 다른 타입을 합치려고 할 경우엔 어떻게 될까?"
date: 2025-09-15 06:35:00 +0900
categories: [Language, Swift]
tags: [swift, type, let, var]
---

서로 다른 타입을 가진 변수를 합치려고 하면 어떻게 될까?

예시
```swift
var example1: String = "Hello World "
var example2: Int = 3

print(example1 + example2)
```

결과
```
Binary operator '+' cannot be applied to operands of type 'String' and 'Int'
```

위와 같은 오류가 발생하게 됨.

Swift에서는 서로 다른 타입을 자동으로 변환해 결합하지 않음.

> Java나 다른 몇몇 언어들은 문자열과 다른 데이터 타입을 결합할 경우 자동으로 문자열 타입으로 만들어 결합해주지만, Swift는 한번 정해진 변수의 타입은 자동으로 변환되지 않음.
{: .prompt-tip }

해결 방법
```swift
var example1: String = "Hello World "
var example2: Int = 3

print(example1 + String(example2))
```

결과
```
Hello World 3
```

위와 같이 결합할 변수와 같은 타입을 가진 새로운 객체를 생성해서 사용해줘야만함.

> 주의할 점은 `String(example2)` 이 코드는 `example2` 변수 값을 문자열로 변환하면서 새로운 문자열 객체를 생성하는 역할을 하는 것임
{: .prompt-tip}