---
title: "Swift - #selector란?"
date: 2025-10-19 15:20:00 +0900
categories: [Language, Swift]
tags: [swift, selector, objc, uikit]
---

## **#selector란?**
`#selector`는 런타임에 특정 메서드를 호출할 수 있도록 참조를 전달하는 문법임.

구문
```swift
#selector(type.method)
```

> `#selector`는 Objective-C에서 사용하던 문법이기 때문에 가리키는 메서드 앞에는 `@objc` 어노테이션을 붙여야함.
{: .prompt-tip }

사용 예시
```swift
import UIKit

class ViewController: UIViewController {
    
    weak var timer: Timer?

    var number: Int = 0
    
    override func viewDidLoad() {
        super.viewDidLoad()
    }
    
    @IBAction func btnTapped(_ sender: UIButton) {
        timer?.invalidate()
        
        timer = Timer.scheduledTimer(
            timeInterval: 1.0,
            target: self,
            selector: #selector(ViewController.exampleSelector),
            userInfo: nil,
            repeats: true
        ) // #selector로 지정한 메서드를 1초마다 실행시킴
    }
    
    @objc func exampleSelector(){
        if number < 60{
          print("1초 지남")
          number += 1
        }else{
          print("끝남")
          timer?.invalidate()
        }
    }
}
```