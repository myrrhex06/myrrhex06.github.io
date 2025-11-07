---
title: "Swift - DispatchQueue global queue를 통한 비동기 처리"
date: 2025-11-07 20:24:00 +0900
categories: [Language, Swift]
tags: [swift, dispatchqueue, async. queue]
---

`DispatcheQueue`는 Swift에서 비동기를 처리하는데 사용하는 객체임.<br>
이 객체의 `global`에 접근하여 `async`를 통해 비동기 처리를 할 수 있음.

`DispatchQueue`는 비동기로 코드를 실행하는 역할을 하는 것이 아니라 `Global Queue`에 작업을 던지는 역할을 하는 것임. <br>

`DispatchQueue`를 통해 `Global Queue`에 작업을 던지면 운영체제(iOS)가 알아서 쓰레드를 할당하여 작업을 비동기적으로 수행해줌.

> `Global Queue`는 동시큐로, 직렬큐와는 다르게 한 쓰레드가 처리하는 것이 아니라 여러 쓰레드로 분산되서 처리됨.
{: .prompt-tip }

사용 예시
```swift
import UIKit

print("작업 시작")

DispatchQueue.global().async {
    print("Task 1")
}

DispatchQueue.global().async {
    print("Task 2")
}

DispatchQueue.global().async {
    print("Task 3")
}

DispatchQueue.global().async {
    print("Task 4")
}

DispatchQueue.global().async {
    print("Task 5")
}

print("작업 종료")
```

결과
```
작업 시작
작업 종료
Task 1
Task 2
Task 3
Task 5
Task 4
```

> 직렬큐는 한 쓰레드에 작업을 순차적으로 실행시키지만, 동시큐는 순서가 없이 여러 쓰레드에 작업을 분산시키기 때문에 실행 순서가 보장되지 않음.
{: .prompt-tip }