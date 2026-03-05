---
title: "UIKIt - UserDefaults 개념 및 사용"
date: 2026-03-05 20:22:00 +0900
categories: [Mobile, iOS]
tags: [ios, uikit, userdefaults, nickname, storage, plist]
---

Apple에서 제공하는 스토리지 중 하나인 `UserDefaults`는 데이터를 `plist`에 저장함.

key-value 형태로 데이터가 저장되기 때문에 `UserDefaults`의 개념이 모호하다 싶으면 브라우저의 로컬 스토리지를 생각하면 쉬움.

> `plist`에 데이터가 저장되기 때문에 민감한 데이터들(ex. JWT 토큰, 비밀번호)을 저장하는 것은 되도록 피해야함.
{: .prompt-tip }

`UserDefaults`를 사용하는 방법은 굉장히 간단함.

예시
```swift
// 데이터 저장
UserDefaults.standard.set("test", forKey: "nickname")

// 데이터 조회
let nickname = UserDefaults.standard.string(forKey: "nickname")
```

현재 프로젝트에서 로그인 후 반환 받은 응답에 닉네임 값이 내려오는데 이 값을 `UserDefaults`에 저장하고 필요할 때 꺼내쓸 생각임.

닉네임은 인증이나 권한과 직접적으로 연결되지 않기 때문에 `UserDefaults`에 저장해도 보안상 큰 문제가 없음.

`LoginViewController.swift`
```swift
import UIKit

final class LoginViewController: UIViewController {

@objc private func loginButtonTapped(){

        let email = handleTextFieldValidate(textField: loginView.getEmailTextField())
        let password = handleTextFieldValidate(textField: loginView.getPasswordTextField())
        
        guard let email = email, let password = password else { return }
        
        authManager.login(email: email, password: password) { [weak self] result in
            guard let self = self else { return }
            
            switch result{
            case .success(let response):
                guard let response = response else { return }
                
                // UserDefaults에 nickname 값 저장
                UserDefaults.standard.set(response.nickname, forKey: Constants.NICKNAME)
                
                ...

            case .failure(let error):
                print("error : \(error)")
            }
        }
    }
}
```

`SettingViewController.swift`
```swift
import UIKit

final class SettingViewController: UIViewController {
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        self.view.backgroundColor = .background

        // UserDefaults에서 값 조회
        let nickname = UserDefaults.standard.string(forKey: Constants.NICKNAME)
        print("nickname : \(nickname)")
    }
}
```

## **Reference**
- [https://julia1281.tistory.com/58](https://julia1281.tistory.com/58)
