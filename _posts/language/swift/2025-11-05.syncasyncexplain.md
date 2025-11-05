---
title: "Swift - 동기와 비동기"
date: 2025-11-05 21:10:00 +0900
categories: [Language, Swift]
tags: [swift, dispatchqueue, async, sync]
---

## **동기**
동기란 작업을 순차적으로 실행하는 것을 뜻함.<br>
작업이 종료될 때까지 다음 작업은 실행되지 않음.

동기 예시
```swift
import UIKit

for row in 1...3{
    print("작업 시작 : \(row)")
    Thread.sleep(forTimeInterval: 1)
    print("작업 종료 : \(row)")
}

print("동기 작업 완료")
```

결과
```
작업 시작 : 1
작업 종료 : 1
작업 시작 : 2
작업 종료 : 2
작업 시작 : 3
작업 종료 : 3
동기 작업 완료
```

## **비동기**
비동기란 작업이 종료될 때까지 기다리지 않고 바로 다음 작업으로 넘어가는 것을 말함.

비동기 예시
```swift
import UIKit

for row in 1...3{
    DispatchQueue.global().async {
        print("작업 시작 : \(row)")
        Thread.sleep(forTimeInterval: 1)
        print("작업 종료 : \(row)")
    }
}

print("비동기 작업 완료")
```

결과
```
비동기 작업 완료
작업 시작 : 2
작업 시작 : 3
작업 시작 : 1
작업 종료 : 1
작업 종료 : 3
작업 종료 : 2
```