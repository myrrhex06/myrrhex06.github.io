---
title: "Swift - 직렬큐 생성 및 사용 방법"
date: 2025-11-06 21:30:00 +0900
categories: [Language, Swift]
tags: [swift, dispatchqueue, async, sync]
---

직렬큐를 사용하게 되면 메인 쓰레드가 처리하지 않고 다른 쓰레드에 작업을 맡기지며, 작업의 순서가 보장됨.

> 직렬큐는 주로 비동기적으로 처리하되 순서를 보장해야하는 상황에서 사용함.
{: .prompt-tip }

예시
```swift
import UIKit

// 직렬큐 생성
let serial = DispatchQueue(label: "serial")

serial.async {
    task1()
}

serial.async {
    task2()
}

serial.async {
    task3()
}


func task1(){
    print("1번 작업")
}


func task2(){
    print("2번 작업")
}


func task3(){
    print("3번 작업")
}
```

결과
```
1번 작업
2번 작업
3번 작업
```