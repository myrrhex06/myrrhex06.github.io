---
title: "UIKIt - present한 NavigationController에서 backButton 설정하기"
date: 2026-03-04 19:02:00 +0900
categories: [Mobile, iOS]
tags: [ios, uikit, navigationcontroller, tabbarcontroller, present, dismiss, leftbutton, navigatinoitem]
---

현재 내가 구현한 탭바는 아래 이미지와 같이 가운데 표시된 버튼을 통해 새로운 책 기록을 등록할 수 있는 화면으로 이동하도록 구현해둠.

![image](/assets/img/currentnavigationcontrollerview.gif)

그래서, 옆으로 넘어가는 `push` 애니메이션보단 아래에서 위로 올라오는 `present`가 훨씬 더 자연스럽게 이어진다고 생각했음.

`TabBarController`에서 아래 코드처럼 책 기록 등록 화면으로 `present`해줌.

`NavigationController`로 감싸서 넘긴 이유는 이전 화면으로 돌아가는 버튼, navigation title을 쉽게 구현할 수 있도록 하기 위함임.

`CustomTabBarController.swift`
```swift
import UIKit

final class CustomTabBarController: UITabBarController {

    private func setCreateButton(){
        let button = UIButton(type: .system)
        let buttonSize: CGFloat = 60
        button.frame = CGRect(x: (tabBar.frame.size.width - buttonSize) / 2, y: -buttonSize / 3, width: buttonSize, height: buttonSize)
        
        let configuration = UIImage.SymbolConfiguration(pointSize: 25)
        let image = UIImage(systemName: "pencil", withConfiguration: configuration)
        
        button.layer.cornerRadius = buttonSize / 2
        
        button.setImage(image, for: .normal)
        button.backgroundColor = .systemBlue
        button.tintColor = .white
        button.addTarget(self, action: #selector(createButtonTapped), for: .touchUpInside)
        tabBar.addSubview(button)
    }
    
    @objc private func createButtonTapped(){
        let createBookVC = UINavigationController(rootViewController: CreateBookViewController())
        createBookVC.modalPresentationStyle = .fullScreen
        
        present(createBookVC, animated: true)
    }

}
```

이제 이전 화면으로 돌아오는 버튼을 넣어줘야하는데 이 경우에는 이동하기 전 화면에서 `backButton`을 설정해줘야하지만 `push`가 아닌 `present` 방식으로 띄웠기 때문에 navigation stack에 의해 자동으로 관리되는 `backButton`이 제대로 적용되지 않는 문제가 발생함.

그래서 이를 우회하기 위해 이전 화면에서 `backButton`을 설정하는 것이 아닌 `CreateBookViewController` 내부에서 `leftButton`을 넣어 `dismiss` 시켜주는 방식으로 구현함.

`CreateBookViewController.swift`
```swift
import UIKit

final class CreateBookViewController: UIViewController {

    override func viewDidLoad() {
        super.viewDidLoad()
        
        self.navigationItem.title = "책 기록 등록"
        self.view.backgroundColor = .background
        
        // navigationItem leftButton 설정
        let backButton = UIBarButtonItem(image: UIImage(systemName: "xmark"), style: .plain, target: self, action: #selector(dismissMainView))
        backButton.tintColor = .white
        self.navigationItem.leftBarButtonItem = backButton
    }
    
    @objc private func dismissMainView(){
        dismiss(animated: true)
    }
}

```

결과


![image](/assets/img/currentnavigationcontrollerleftbuttonsettingresult.gif)

