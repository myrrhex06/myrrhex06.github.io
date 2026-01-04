---
title: "UIKit - UIViewController 생명주기(라이프사이클)"
date: 2025-10-27 19:45:00 +0900
categories: [Mobile, iOS]
tags: [uikit, storyboard, code, uiviewcontroller, present, dismiss]
---

## **UIViewController 생명주기(라이프사이클)**

### **Apple Develop Document**
![image](/assets/img/viewcontrollerlifecyclebyuikitdocument.png)


| 메서드명              | 설명                                                                                                                  |
| --------------------- | --------------------------------------------------------------------------------------------------------------------- |
| **loadView**          | 뷰가 메모리에 로드되기 전에 호출됨<br>(스토리보드로 구성했을 경우 `loadView`를 오버라이드하면 화면이 렌더링되지 않음) |
| **viewDidLoad**       | 뷰가 메모리에 로드된 이후에 호출됨                                                                                    |
| **viewWillAppear**    | 실제 화면에 뷰가 나타나기 전에 호출됨                                                                                 |
| **viewIsAppearing**   | `viewWillAppear`과 `viewDidAppear` 사이에서, 뷰가 이미 계층 구조에 추가되고 실제 화면에 렌더링되기 직전에 호출됨      |
| **viewDidAppear**     | 실제 화면에 뷰가 나타난 후에 호출됨                                                                                   |
| **viewWillDisappear** | 실제 화면에서 뷰가 사라지기 전에 호출됨                                                                               |
| **viewDidDisappear**  | 실제 화면에서 뷰가 사라진 후에 호출됨                                                                                 |


### **사용 예시**
`BasicView.swift`
```swift
import UIKit

class BasicView: UIView {
    
    let exBtn: UIButton = {
        let btn = UIButton()
        
        btn.setTitle("클릭", for: .normal)
        btn.setTitleColor(.white, for: .normal)
        btn.backgroundColor = .black
        btn.titleLabel?.font = .systemFont(ofSize: 20, weight: .bold)
        
        return btn
    }()
    
    override init(frame: CGRect) {
        super.init(frame: frame)
        configureUI()
    }
    
    required init?(coder: NSCoder) {
        super.init(coder: coder)
        configureUI()
        fatalError()
    }
    
    func configureUI(){
        addSubview(exBtn)
        
        backgroundColor = .white
        
        exBtn.translatesAutoresizingMaskIntoConstraints = false
        exBtn.centerXAnchor.constraint(equalTo: centerXAnchor).isActive = true
        exBtn.centerYAnchor.constraint(equalTo: centerYAnchor).isActive = true
        exBtn.widthAnchor.constraint(equalToConstant: 60).isActive = true
        exBtn.heightAnchor.constraint(equalToConstant: 50).isActive = true
    }
}
```

`ViewController.swift`
```swift
import UIKit

class ViewController: UIViewController {
    
    private let basicView = BasicView()
    
    // 뷰가 메모리에 로드되기 전에 호출
    override func loadView() {
        self.view = basicView
        print("ViewController - loadView 실행")
    }

    // 뷰가 메모리에 올라온 후에 호출
    override func viewDidLoad() {
        super.viewDidLoad()
        print("ViewController - viewDidLoad 실행")
    }
    
    // 실제 화면에 뷰가 나타나기 전에 호출됨
    override func viewWillAppear(_ animated: Bool) {
        super.viewWillAppear(animated)
        print("ViewController - viewWillAppear 실행")
    }
    
    // viewWillAppear과 viewDidAppear 사이에서, 뷰가 이미 계층 구조에 추가되고 실제 화면에 렌더링되기 직전에 호출됨
    override func viewIsAppearing(_ animated: Bool) {
        super.viewIsAppearing(animated)
        print("ViewController - viewIsAppearing 실행")
    }
    
    // 실제 화면에 뷰가 나타난 후에 호출됨
    override func viewDidAppear(_ animated: Bool) {
        super.viewDidAppear(animated)
        print("ViewController - viewDidAppear 실행")
    }
    
    // 실제 화면에서 뷰가 사라지기 전에 호출됨
    override func viewWillDisappear(_ animated: Bool) {
        super.viewWillDisappear(animated)
        print("ViewController - viewWillDisappear 실행")
    }
    
    // 실제 화면에서 뷰가 사라진 후에 호출됨
    override func viewDidDisappear(_ animated: Bool) {
        super.viewDidDisappear(animated)
        print("ViewController - viewDidDisappear 실행")
    }
}
```