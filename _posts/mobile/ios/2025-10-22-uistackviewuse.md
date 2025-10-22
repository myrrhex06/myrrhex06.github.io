---
title: "UIStackView 사용 방법"
date: 2025-10-22 20:46:00 +0900
categories: [Mobile, iOS]
tags: [uikit, code, uibutton, uistackview]
---

## **UIStackView란?**
`UIStackView`는 각각의 `View`들을 하나로 묶어주는 역할을 함.

### **Apple Develop Document**
`UIStackView` 소개

![image](/assets/img/uistackviewdocument1.png)

제공 생성자

![image](/assets/img/uistackviewdocument2.png)

사용 가능 프로퍼티 (일부만 가져옴)

![image](/assets/img/uistackviewdocument3.png)

- `axis`: 정렬할 방향
- `alignment`: 스택뷰 내부 정렬
- `distribution`: 스택뷰 내부 공간 분배 기준
- `spacing`: 각 뷰의 간격

### **구현 예시**
```swift
import UIKit

final class ViewController: UIViewController {
    
    private let exBtn1: UIButton = {
        let btn = UIButton()
        
        btn.setTitle("exBtn1", for: .normal)
        btn.backgroundColor = .black
        btn.setTitleColor(.white, for: .normal)
        
        return btn
    }()
    
    private let exBtn2: UIButton = {
        let btn = UIButton()
        
        btn.setTitle("exBtn2", for: .normal)
        btn.backgroundColor = .black
        btn.setTitleColor(.white, for: .normal)
        
        return btn
    }()
    
    private let exBtn3: UIButton = {
        let btn = UIButton()
        
        btn.setTitle("exBtn3", for: .normal)
        btn.backgroundColor = .black
        btn.setTitleColor(.white, for: .normal)
        
        return btn
    }()
    
    // StackView
    private lazy var exStackView: UIStackView = {
        let stackView = UIStackView(arrangedSubviews: [exBtn1, exBtn2, exBtn3])
        
        stackView.axis = .vertical // 세로축
        stackView.spacing = 10 // 간격
        stackView.alignment = .center // 가운데 정렬
        stackView.distribution = .fillEqually // 모두 동일한 공간을 받음
        stackView.backgroundColor = .red
        
        return stackView
    }()

    override func viewDidLoad() {
        super.viewDidLoad()
        
        self.view.addSubview(exStackView)
        
        setupUI()
    }
    
    func setupUI(){
        exStackView.translatesAutoresizingMaskIntoConstraints = false
        
        NSLayoutConstraint.activate([
            exStackView.centerYAnchor.constraint(equalTo: self.view.centerYAnchor),
            exStackView.leadingAnchor.constraint(equalTo: self.view.leadingAnchor, constant: 30),
            exStackView.trailingAnchor.constraint(equalTo: self.view.trailingAnchor, constant: -30)
        ])
    }

}
```

**결과**

![image](/assets/img/uistackviewresult.png)