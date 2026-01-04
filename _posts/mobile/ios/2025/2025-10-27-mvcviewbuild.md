---
title: "UIKit - MVC 패턴 View 구현"
date: 2025-10-27 21:03:00 +0900
categories: [Mobile, iOS]
tags: [uikit, code, uiview]
---

## **MVC 패턴 View 구현**
MVC(Model View Controller) 패턴에서 화면을 담당하는 `View`를 구현할 떄는 `UIView` 클래스를 상속해서 처리해야함.

`BasicView.swift`
```swift
import UIKit

class BasicView: UIView {
    
    private let exLabel: UILabel = {
        let label = UILabel()
        
        label.text = "Hello World!"
        
        return label
    }()
    
    override init(frame: CGRect) {
        super.init(frame: frame)
        setupUI()
        setupConstraint()
    }
    
    required init?(coder: NSCoder) {
        super.init(coder: coder)
        fatalError()
    }
    
    func setupUI(){
        
        self.backgroundColor = .white
        
        addSubview(exLabel)
    }
    
    func setupConstraint(){
        exLabel.translatesAutoresizingMaskIntoConstraints = false
        
        exLabel.centerXAnchor.constraint(equalTo: self.centerXAnchor).isActive = true
        exLabel.centerYAnchor.constraint(equalTo: self.centerYAnchor).isActive = true
    }
}
```

이때 두 가지 생성자를 필수적으로 구현해줘야함.

```swift
// 코드로 뷰를 만들 때 사용되는 생성자
override init(frame: CGRect) {
  super.init(frame: frame)
  setupUI()
  setupConstraint()
}
    
// 스토리보드로 뷰를 만들 때 사용되는 생성자
required init?(coder: NSCoder) {
  super.init(coder: coder)
  fatalError() // 코드로만 구현했을 경우 호출될 일이 없기 때문에 만약 호출될 경우 앱이 죽도록 구성
}
```

> `UIView`를 상속받은 커스텀 뷰는 코드로 생성될 수도 있고, 스토리보드에서 생성될 수도 있기 때문에 두 생성자를 모두 구현해야 함.
{: .prompt-tip }

`ViewController.swift`
```swift
import UIKit

class ViewController: UIViewController {

    let basicView = BasicView()
    
    override func loadView() {
        self.view = basicView
    }
    
    override func viewDidLoad() {
        super.viewDidLoad()
    }
}
```

`loadView()`를 오버라이드해서 `ViewController`의 루트 뷰를 직접 교체하면, 커스텀 `UIView`가 메모리에 로드될 때 자동으로 화면에 표시됨.

결과

![image](/assets/img/mvcviewresult.png)

