---
title: "UIKit - Frame과 Bounds란?"
date: 2025-12-10 21:52:00 +0900
categories: [Mobile, iOS]
tags: [uikit, code, uiview, frame, bounds]
---

> [이 글](https://myrrhex06.github.io/posts/cgpointcgsizecgrect)과 이어지는 내용임.

## **Frame**

`frame`은 부모 뷰(`superview`)를 기준으로 어떤 뷰가 어떤 위치(`origin`)에 있고 얼만큼의 공간(`size`)을 차지하고 있는지 나타냄.

`ViewController.swift`
```swift
import UIKit

class ViewController: UIViewController {

    private let uiView = {
        
        let rect = CGRect(x: 100, y: 200, width: 200, height: 200)
        
        // frame 설정
        let view = UIView(frame: rect)
        
        view.backgroundColor = .green
        
        return view
        
    }()

    override func viewDidLoad() {
        super.viewDidLoad()
        
        self.view.addSubview(uiView)
        
        print("frame : \(uiView.frame)")
    }
}
```

출력 결과
```
frame : (100.0, 200.0, 200.0, 200.0)
```

- `origin`: 부모 뷰의 원점(0, 0)을 기준으로 얼마나 떨어져 있는지를 나타냄
- `size`: 뷰가 실제로 차지하는 직사각형의 크기

> `transform`(회전, 스케일) 등을 적용하면, 뷰가 차지하는 물리적인 영역이 달라지기 때문에 `frame.size`는 바뀔 수 있음.
{: .prompt-tip }

## **Bounds**

`bounds`는 자기 자신을 기준으로 하는 내부 좌표계임.

`ViewController.swift`
```swift
import UIKit

class ViewController: UIViewController {

    private let uiView = {
        
        let rect = CGRect(x: 100, y: 200, width: 200, height: 200)
        
        // frame 설정
        let view = UIView(frame: rect)
        
        view.backgroundColor = .green
        
        return view
        
    }()

    override func viewDidLoad() {
        super.viewDidLoad()
        
        self.view.addSubview(uiView)
        
        print("frame : \(uiView.bounds)")
    }
}
```

출력 결과
```
frame : (0.0, 0.0, 200.0, 200.0)
```

- `origin`: 항상 자기 자신이 기준이기 때문에 기본값(0, 0)
- `size`: 해당 뷰의 본래 크기

> `transform`을 해도 `bounds.size`는 그대로 유지됨.
{: .prompt-tip }

### **Bounds의 origin 변경**

`bounds`의 `origin`을 변경해도 뷰 자체는 움직이지 않음.

대신, 뷰가 보여질 영역(`viewport`)이 움직이게 됨.

즉, 내부 콘텐츠를 어느 부분부터 보여줄지 결정하는 값임.

대표적인 예시로는 `UIScrollView`가 있음.

`UIScrollView`는 이 `bounds.origin`을 통해 화면을 스크롤할 때 origin 값을 변경하여 실제로 화면에 표시되는 콘텐츠를 제어함.

## **Reference**
- [https://babbab2.tistory.com/46](https://babbab2.tistory.com/46)
- [https://babbab2.tistory.com/45](https://babbab2.tistory.com/45)
- [https://babbab2.tistory.com/44](https://babbab2.tistory.com/44)