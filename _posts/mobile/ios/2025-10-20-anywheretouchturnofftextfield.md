---
title: "UIKit - 화면의 아무 곳이나 터치했을 때 키패드 내려가게 하는 방법"
date: 2025-10-20 20:35:00 +0900
categories: [Mobile, iOS]
tags: [uikit, storyboard, textfield]
---

## **화면의 아무 곳이나 터치했을 때 키패드 내려가게 하는 방법**
`UIVIewController`에 정의되어 있는 `touchesBegan` 메서드를 오버라이딩하여 처리가 가능함.

`touchesBegan` 메서드는 뷰 컨트롤러가 화면의 터치를 감지할 때 자동으로 호출되기 때문에,  
터치가 발생하면 키보드를 내려주는 동작을 손쉽게 구현할 수 있음.

예시
```swift
import UIKit

class ViewController: UIViewController, UITextFieldDelegate {

    // 화면의 탭을 감지하는 메서드
    override func touchesBegan(_ touches: Set<UITouch>, with event: UIEvent?) {
        // 아무 곳이나 터치했을 때 키패드가 내려가도록 설정
        self.view.endEditing(true)
    }
}
```
