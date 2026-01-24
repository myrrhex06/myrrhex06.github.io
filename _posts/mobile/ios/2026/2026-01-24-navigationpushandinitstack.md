---
title: "UIKit - NavigationController 푸시 후에 스택 비우기"
date: 2026-01-24 10:25:00 +0900
categories: [Mobile, iOS]
tags: [ios, uikit, navigationcontroller, push]
---

사이드 프로젝트를 개발하면서 로그인 후에 메인 화면으로 이동될 때 아래 화면처럼 `backButton`이 표시됨.

![image](/assets/img/afterlogindisplaybackbutton.gif)

이 문제를 해결하기 위해서 아래 코드를 통해 로그인 성공 시 메인 화면으로 푸시하면서 스택을 초기화 시켜줌.

```swift
authManager.login(email: email, password: password) { result in
  switch result{
    case .success(let response):
      guard let response = response else { return }
                  
      KeyChainUtil.create(key: Constants.ACCESS_TOKEN, data: response.accessToken)
      KeyChainUtil.create(key: Constants.REFRESH_TOKEN, data: response.refreshToken)
                  
      // UI 작업이기 때문에 Main Thread에서 실행
      DispatchQueue.main.async {
        // setViewControllers를 통해 Stack 초기화 후 push
        self.navigationController?.setViewControllers([HomeViewController()], animated: true)
      }
                  
    case .failure(let error):
      print("error : \(error)")
  }
}
```

결과

![image](/assets/img/afterloginresult.gif)

