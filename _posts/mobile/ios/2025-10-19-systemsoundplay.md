---
title: "Apple에서 제공되는 사운드 재생 방법"
date: 2025-10-19 14:46:00 +0900
categories: [Mobile, iOS]
tags: [uikit, storyboard, avfoundation, sound]
---

## **Apple에서 제공되는 사운드 재생 방법**

`AVFoundation`을 통해 Apple에서 기본으로 제공해주는 사운드를 재생할 수 있음.

> `AVFoundation`이란 Apple 플랫폼에서 오디오 재생, 캡처 등 다양한 기능을 쉽게 구현할 수 있도록 제공해주는 미디어 프레임워크임.
{: .prompt-tip }

**Apple Develop Document**
![image](/assets/img/soundPlayFounctionDocument.png)

시스템 사운드를 재생하는 메서드

구현 예시 코드
```swift
import UIKit
import AVFoundation // AVFoundation import

class ViewController: UIViewController {
    
    override func viewDidLoad() {
        super.viewDidLoad()
    }

    @IBAction func btnTapped(_ sender: UIButton) {
      AudioServicesPlayAlertSound(SystemSoundID(1322)) // 사운드 재생
    }
}
```
