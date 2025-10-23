---
title: "UIKit - 화면 전환 및 데이터 전달(코드로 구현)"
date: 2025-10-23 21:33:00 +0900
categories: [Mobile, iOS]
tags: [uikit, code, present, dismiss, viewcontroller]
---

## **화면 전환 및 데이터 전달(코드로 구현)**
`UIViewController`의 정의되어 있는 `present`, `dismiss` 메서드를 통해 구현 가능함.

### **Apple Develop Document**
![image](/assets/img/presentdismissdocument.png)

- `present`: 화면 전환 역할을 담당
- `dismiss`: 이전 화면으로 돌아가는 역할을 담당

### **구현 예시**
`ViewController.swift`
```swift
import UIKit

final class ViewController: UIViewController {
    
    private lazy var exBtn: UIButton = {
        let btn = UIButton()
        
        btn.setTitle("화면 이동", for: .normal)
        btn.backgroundColor = .black
        btn.setTitleColor(.white, for: .normal)
        btn.addTarget(self, action: #selector(exBtnTapped), for: .touchUpInside)
        
        return btn
    }()

    override func viewDidLoad() {
        super.viewDidLoad()
        
        setupUI()
    }
    
    func setupUI() {
        view.addSubview(exBtn)
        
        exBtn.translatesAutoresizingMaskIntoConstraints = false
        
        NSLayoutConstraint.activate([
            exBtn.centerXAnchor.constraint(equalTo: view.centerXAnchor),
            exBtn.centerYAnchor.constraint(equalTo: view.centerYAnchor),
            exBtn.widthAnchor.constraint(equalToConstant: 100),
            exBtn.heightAnchor.constraint(equalToConstant: 50)
        ])
    }
    
    @objc func exBtnTapped(){
        
        let convertVC = ConvertViewController()
        
        convertVC.exValue = "Hello World!" // 데이터 전달
        convertVC.modalPresentationStyle = .fullScreen // 화면 디자인 설정
        
        // 화면을 전환하는 역할을 수행함
        present(convertVC, animated: true)
    }
}
```

`ConvertViewController.swift`
```swift
import UIKit

final class ConvertViewController: UIViewController {

    private lazy var rollBackBtn : UIButton = {
        let btn = UIButton()
    
        btn.setTitle("뒤로가기", for: .normal)
        btn.backgroundColor = .black
        btn.setTitleColor(.white, for: .normal)
        btn.addTarget(self, action: #selector(rollBackBtnTapped), for: .touchUpInside)
        
        return btn
    }()
    
    private lazy var exValueLabel: UILabel = {
        let label = UILabel()

        label.font = UIFont.boldSystemFont(ofSize: 22)
        
        return label
    }()
    
    // 데이터를 전달받기 위한 변수
    var exValue: String?
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        setupUI()
        exValueLabel.text = exValue // 넘어온 데이터 할당
    }
    
    func setupUI(){
        view.backgroundColor = .gray
        
        view.addSubview(rollBackBtn)
        view.addSubview(exValueLabel)
        
        rollBackBtn.translatesAutoresizingMaskIntoConstraints = false
        exValueLabel.translatesAutoresizingMaskIntoConstraints = false
        
        NSLayoutConstraint.activate([
            rollBackBtn.centerXAnchor.constraint(equalTo: view.centerXAnchor),
            rollBackBtn.bottomAnchor.constraint(equalTo: view.bottomAnchor, constant: -50),
            rollBackBtn.widthAnchor.constraint(equalToConstant: 100),
            rollBackBtn.heightAnchor.constraint(equalToConstant: 50),
            
            exValueLabel.centerXAnchor.constraint(equalTo: view.centerXAnchor),
            exValueLabel.centerYAnchor.constraint(equalTo: view.centerYAnchor)
        ])
    }
    
    @objc func rollBackBtnTapped(){
        
        // 이전 화면으로 되돌아가게 하는 역할을 수행함
        dismiss(animated: true)
    }

}
```

위 예시 코드는 `ViewController`에서 버튼을 클릭하면 `ConvertViewController` 화면으로 전환되며, `exValue` 변수를 통해 데이터를 전달받음.

또한, `modalPresentationStyle`라는 `UIViewController`의 정의되어 있는 프로퍼티 값을 설정하여 화면이 전환될 때 어떤 형태(ex. 모달, 풀스크린 등)로 전환될지를 설정할 수 있음.