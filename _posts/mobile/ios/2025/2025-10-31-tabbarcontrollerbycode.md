---
title: "UIKit - TabBarController 설정 방법 (코드)"
date: 2025-10-31 06:38:00 +0900
categories: [Mobile, iOS]
tags: [uikit, code, tabbarcontroller]
---

## **TabBarController 설정 방법 (코드)**
`ViewController.swift`
```swift
import UIKit

class ViewController: UIViewController {
    
    private lazy var btn: UIButton = {
        let btn = UIButton()
        
        btn.setTitle("다음으로", for: .normal)
        btn.setTitleColor(.white, for: .normal)
        btn.backgroundColor = .black
        
        btn.translatesAutoresizingMaskIntoConstraints = false
        
        btn.addTarget(self, action: #selector(btnTapped), for: .touchUpInside)
        
        return btn
    }()

    override func viewDidLoad() {
        super.viewDidLoad()
        
        makeUI()
    }
    
    func makeUI(){
        view.addSubview(btn)
        
        NSLayoutConstraint.activate([
            btn.centerXAnchor.constraint(equalTo: view.centerXAnchor),
            btn.centerYAnchor.constraint(equalTo: view.centerYAnchor),
            btn.widthAnchor.constraint(equalToConstant: 150),
            btn.heightAnchor.constraint(equalToConstant: 50)
        ])
    }
    
    @objc func btnTapped(){
        let tab = UITabBarController()
        
        let vc1 = FirstViewController()
        let vc2 = SecondViewController()
        
        // TabBar Title 설정
        vc1.title = "Home"
        vc2.title = "Profile"
        
        // TabBar ViewController 설정
        tab.setViewControllers([vc1, vc2], animated: true)
        
        // present 스타일 설정
        tab.modalPresentationStyle = .fullScreen
        
        // TabBar 배경색 설정
        tab.tabBar.backgroundColor = .white
        
        // items 배열 옵셔널 바인딩
        guard let items = tab.tabBar.items else {
            return
        }
        
        // TabBar 이미지 설정
        items[0].image = UIImage(systemName: "house")
        items[1].image = UIImage(systemName: "person.fill")
        
        present(tab, animated: true)
    }
}
```

`FirstViewController.swift`
```swift
import UIKit

class FirstViewController: UIViewController {

    override func viewDidLoad() {
        super.viewDidLoad()

        view.backgroundColor = .red
    }
}
```

`SecondViewController.swift`
```swift
import UIKit

class SecondViewController: UIViewController {

    override func viewDidLoad() {
        super.viewDidLoad()

        view.backgroundColor = .blue
    }
}
```

결과

![image](/assets/img/tabbarcontrollerbycoderesult.gif)