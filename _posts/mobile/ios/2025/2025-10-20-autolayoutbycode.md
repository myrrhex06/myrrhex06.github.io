---
title: "UIKit - 오토 레이아웃(Auto Layout) 설정 방법"
date: 2025-10-20 21:14:00 +0900
categories: [Mobile, iOS]
tags: [uikit, code, textfield]
---

## **오토 레이아웃(Auto Layout) 설정 방법**
스토리보드가 아닌 코드를 통해 오토 레이아웃을 설정하는 방법은 아래와 같음.

예시
```swift
import UIKit

class ViewController: UIViewController {
    
    private let exBtn: UIButton = {
        let btn = UIButton(type: .custom)
        
        btn.setTitle("Hello", for: .normal)
        btn.backgroundColor = .black
        btn.setTitleColor(.white, for: .normal)
        
        return btn
    }()

    override func viewDidLoad() {
        super.viewDidLoad()
        
        setupUI()
    }
    
    func setupUI(){
        self.view.addSubview(exBtn)
        
        // 자동 프레임 조정 해제
        exBtn.translatesAutoresizingMaskIntoConstraints = false

        // 오토레이아웃 설정
        exBtn.leadingAnchor.constraint(equalTo: self.view.leadingAnchor, constant: 20).isActive = true
        exBtn.trailingAnchor.constraint(equalTo: self.view.trailingAnchor, constant: -20).isActive = true
        exBtn.centerYAnchor.constraint(equalTo: self.view.centerYAnchor).isActive = true
    }
}
```

예시를 천천히 분석해보겠음.

```swift
import UIKit

class ViewController: UIViewController {
    private let exBtn: UIButton = {
        let btn = UIButton(type: .custom)
        
        btn.setTitle("Hello", for: .normal)
        btn.backgroundColor = .black
        btn.setTitleColor(.white, for: .normal)
        
        return btn
    }()

    ...
}
```

가정 첫번째에 있는 `exBtn`은 `UIButton`으로, 스토리보드에서는 직접 버튼을 끌어다가 화면에 올리면 됬지만, 코드로 구현할 때는 세부적인 것들을 전부 설정해줘야함.

위 코드처럼 설정을 해줬다고 해서 바로 화면에 나타나는 것이 아님.

```swift
import UIKit

class ViewController: UIViewController {
    
    ...

    override func viewDidLoad() {
        super.viewDidLoad()
        
        setupUI()
    }
    
    func setupUI(){
        self.view.addSubview(exBtn)
        
        ...
    }
}
```

위 코드에서 `addSubView` 메서드를 통해서 `UIViewController`에 선언되어 있는 `view`의 하위 뷰로 넣어줘야함.

아직 끝난 것이 아님.

버튼이 제대로 렌더링 되기 위해서는 제약이 필요함.

```swift
// 자동 프레임 조정 해제
exBtn.translatesAutoresizingMaskIntoConstraints = false
```

오토 레이아웃을 설정하기 전에는 `translatesAutoresizingMaskIntoConstraints` 프로퍼티 값을 `false`로 설정해서 자동 프레임 조정 설정을 해제해줘야함.

자동 프레임 조정 설정을 `false`로 하지 않으면 기존 `frame` 기반의 자동 제약(Autoresizing Mask)을 오토레이아웃 제약으로 변환하는 기능을 그대로 사용하게 되고, 오토 레이아웃 제약 설정과 충돌이 나기 때문에 `false`로 지정해줘야함.

> 자동 제약(Autoresizing Mask)은 오토 레이아웃 등장 전 UIKit에서 부모 `view` 크기 변화에 따라 자식 `view`의 위치와 크기를 자동으로 조정해주는 자동 변환 기능임.
{: .prompt-tip }

```swift
// 오토레이아웃 설정
exBtn.leadingAnchor.constraint(equalTo: self.view.leadingAnchor, constant: 20).isActive = true // 앞쪽(왼쪽)
exBtn.trailingAnchor.constraint(equalTo: self.view.trailingAnchor, constant: -20).isActive = true // 뒤쪽(오른쪽)
exBtn.centerYAnchor.constraint(equalTo: self.view.centerYAnchor).isActive = true // Y축의 가운데 설정
```

위와 같이 `leadingAnchor`, `trailingAnchor`로 어느 곳에 제약을 걸지 방향을 지정한 후 `constraint` 메서드를 통해 오토 레이아웃을 설정할 수 있음.

결과

![image](/assets/img/autolayoutbycoderesult.png)

설정된 제약들이 잘 적용된 것을 확인할 수 있음.

위 코드처럼 오토 레이아웃을 지정하는 방법 외에도 다른 방법을 통해서 오토 레이아웃을 설정할 수 있음.

예시
```swift
import UIKit

class ViewController: UIViewController {
    
    private let exBtn: UIButton = {
        let btn = UIButton(type: .custom)
        
        btn.setTitle("Hello", for: .normal)
        btn.backgroundColor = .black
        btn.setTitleColor(.white, for: .normal)
        
        return btn
    }()

    override func viewDidLoad() {
        super.viewDidLoad()
        
        setupUI()
    }
    
    func setupUI(){
        self.view.addSubview(exBtn)
        
        // 자동 프레임 조정 해제
        // 자동 프레임 조정 설정을 false로 하지 않으면 오토 레이아웃이 먹히지 않음
        exBtn.translatesAutoresizingMaskIntoConstraints = false

        // 오토레이아웃 설정
        NSLayoutConstraint.activate([
            exBtn.leadingAnchor.constraint(equalTo: self.view.leadingAnchor, constant: 20),
            exBtn.trailingAnchor.constraint(equalTo: self.view.trailingAnchor, constant: -20),
            exBtn.centerYAnchor.constraint(equalTo: self.view.centerYAnchor)
        ])
    }
}
```

위 코드에서는 `NSLayoutConstraint`의 `activate` 메서드를 통해서 오토 레이아웃을 설정하며 결과는 동일함.