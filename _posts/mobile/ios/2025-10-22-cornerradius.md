---
title: "UIView Corner Radius 적용 방법"
date: 2025-10-22 21:43:00 +0900
categories: [Mobile, iOS]
tags: [uikit, code, uiview, radius]
---

## **Corner Radius 적용 방법**
`UIView`의 `layer`의 `cornerRadius`, `masksToBounds`를 통해 설정 가능함.

> `masksToBounds`는 `radius` 설정으로 인해 `UiView`의 모서리가 둥굴어지는데 이때 서브뷰나 레이어가 모서리 밖으로 나가지 않도록 잘라주는 역할을 함.
{: .prompt-tip }

적용 예시 코드
```swift
import UIKit

final class ViewController: UIViewController {
    
    private lazy var exView: UIView = {
       let view = UIView()
        
        // Radius 설정
        view.layer.masksToBounds = true
        view.layer.cornerRadius = 10
        
        return view
    }()

    ...
}
```