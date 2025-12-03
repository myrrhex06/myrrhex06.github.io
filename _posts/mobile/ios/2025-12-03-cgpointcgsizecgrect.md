---
title: "UIKit - CGPoint, CGSize, CGRect란?"
date: 2025-12-04 06:39:00 +0900
categories: [Mobile, iOS]
tags: [uikit, code, cgpoint, cgsize, cgrect]
---

## CGPoint란?

x축, y축 좌표를 표현하는데 사용함.

> 주로 `view`에 위치를 설정할 때 사용하지만, x축과 y축을 표현할 땐 어느곳에서나 사용 가능함.
{: .prompt-tip }

iOS에서는 아래 이미지처럼 기기 화면의 왼쪽 위가 원점(0, 0)임.

![image](/assets/img/deviceoriginxy.png)

`CGPoint` 구조체
```swift
public struct CGPoint {

    public init()

    public init(x: Double, y: Double)

    public var x: Double

    public var y: Double
}
```

사용 예시
```swift
let origin = CGPoint(x: 100, y: 200)
```

## CGSize란?

너비(`width`), 높이(`height`)를 표현하는데 사용함.

`CGSize` 구조체
```swift
public struct CGSize {

    public init()

    public init(width: Double, height: Double)

    public var width: Double

    public var height: Double
}
```

사용 예시
```swift
let size = CGSize(width: 200, height: 200)
```

## CGRect란?

사각형의 위치(`origin`), 넓이(`size`)를 표현하는데 사용함.

> iOS에서는 넓이(`width`, `height`) 뿐만 아니라 위치(`x`, `y`)에 대한 정보까지 있어야 view를 그릴 수 있음.
{: .prompt-tip }

`CGRect` 구조체
```swift
public struct CGRect {

    public init()

    public init(origin: CGPoint, size: CGSize)

    public var origin: CGPoint

    public var size: CGSize
}
```

위에서 먼저 다룬 `CGPoint`, `CGSize` 타입으로 선언된 프로퍼티를 확인할 수 있음.

해당 프로퍼티를 통해 x축과 y축(`CGPoint`), 너비와 높이(`CGSize`)를 설정할 수 있음.

사용 예시
```swift
import UIKit

class ViewController: UIViewController {
    
    let exView: UIView = {
        let origin = CGPoint(x: 100, y: 200)
        let size = CGSize(width: 200, height: 200)
        let rect = CGRect(origin: origin, size: size)
        
        let view = UIView(frame: rect)
        
        view.backgroundColor = .red
        
        return view
    }()
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        self.view.addSubview(exView)
    }
}
```

결과

![image](/assets/img/cgrectapply.png)

위 결과에서 확인할 수 있듯이 원점(0, 0)에서 x축으로 100, y축으로 200만큼 떨어져 배치된 것을 확인할 수 있음.

## Reference
- [https://zeddios.tistory.com/201](https://zeddios.tistory.com/201)
- [https://babbab2.tistory.com/42](https://babbab2.tistory.com/42)