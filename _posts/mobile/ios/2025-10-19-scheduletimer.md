---
title: "정해진 시간마다 동작하는 Timer 구현 방법"
date: 2025-10-19 14:30:00 +0900
categories: [Mobile, iOS]
tags: [uikit, storyboard, timer]
---

## **정해진 시간마다 동작하는 Timer 구현 방법**

`Foundation`에서 제공하는 `Timer` 클래스를 통해 구현 가능함.

**Apple Develop Document**

타이머 설정 메서드
![image](/assets/img/documentationtimer.png)

타이머를 만들고 바로 실행되도록 현재 스레드의 RunLoop에 등록

타이머 제거 메서드
![image](/assets/img/documentationstoptimer.png)

런루프에 등록되어 있는 타이머 제거

사용 예시 코드
```swift
import UIKit

class ViewController: UIViewController {
    weak var timer: Timer?
    var value: Int = 0

    func example() {
        // 1초마다 실행되는 타이머 생성 및 현재 RunLoop에 등록
        timer = Timer.scheduledTimer(withTimeInterval: 1.0, repeats: true) { [self] _ in
            if value < 60 {
                print("1초마다 실행됨.")
                value += 1
            } else {
                print("종료")
                timer?.invalidate() // 타이머 제거
            }
        }
    }
}
```