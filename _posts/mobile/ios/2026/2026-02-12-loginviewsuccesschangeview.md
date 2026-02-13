---
title: "UIKit - 로그인 성공 시 화면 전환하기"
date: 2026-02-12 06:46:00 +0900
categories: [Mobile, iOS]
tags: [ios, uikit, viewcontroller, scenedelegate, navigationcontroller]
---

기존에 구현되어 있던 로직으로는 아래 화면과 같이 로그인 성공 시 메인 화면으로 푸시하여 화면 전환을 처리하고 있었음.

![image](/assets/img/beforechangingloginsuccesspush.gif)

위와 같이 처리했을 경우 기능상에는 문제가 없으나 화면에 보이는 것 처럼 네비게이션 바가 2개가 생기기 때문에 네비게이션 바 영역이 일반적인 크기보다 더 크게 나타나는 것을 알 수 있음.

> [네비게이션 바 겹침 현상 해결](https://myrrhex06.github.io/posts/duplicatenavigationbar/) 참고
{: .prompt-tip }

이 문제를 해결하기 위해 로그인 성공 시 네비게이션 푸시 대신 `SceneDelegate`에서 `rootViewController`를 변경하는 메서드를 새로 추가하여 해당 메서드를 통해 화면 전환을 처리하도록 변경함.

`SceneDelegate.swift`
```swift
import UIKit

class SceneDelegate: UIResponder, UIWindowSceneDelegate {

    var window: UIWindow?

    func changeRootViewController(viewController: UIViewController){
      guard let window = window else { return }
        
      UIView.transition(with: window, duration: 0.3, options: .transitionCrossDissolve) {
          window.rootViewController = viewController
          window.makeKeyAndVisible()
      }
    }
}
```

`LoginViewController.swift`
```swift
import UIKit

final class LoginViewController: UIViewController {
    
    private let loginView = LoginView()
    
    private let authManager = AuthManager.shared

    ...

    @objc private func loginButtonTapped(){
        let email = handleTextFieldValidate(textField: loginView.getEmailTextField())
        let password = handleTextFieldValidate(textField: loginView.getPasswordTextField())
        
        guard let email = email, let password = password else { return }
        
        authManager.login(email: email, password: password) { result in
            switch result{
            case .success(let response):
                ...
                
                DispatchQueue.main.async {
                  // SceneDelegate 조회
                    guard let sceneDelegate = self.view.window?.windowScene?.delegate as? SceneDelegate else { return }

                    // rootView 변경 처리
                    sceneDelegate.changeRootViewController(viewController: CustomTabBarController())
                }
                
            case .failure(let error):
                print("error : \(error)")
            }
        }
    }
}
```

결과

![image](/assets/img/afterchangingloginsuccesspush.gif)