---
title: "Troubleshooting - NavigationBar 겹침 현상 해결"
date: 2026-02-07 16:47:00 +0900
categories: [Troubleshooting]
tags: [xcode, uiview, navigationbar, debug]
---

## **이슈**
아래 화면과 같이 `NavigationBar` 영역이 엄청 크게 잡히는 것을 확인할 수 있음.

![image](/assets/img/duplicatenavigationbar.gif)

## **원인**

`View` 계층 구조를 확인해본 결과 아래와 같이 `NavigationBar`가 2개가 생성되어 있음.

<img src="/assets/img/debuggingduplicatenavigationbar.png" alt="image" width="500">

기억을 되짚어 코드를 찾아보던 도중 아래와 같이 `SceneDelegate`에서 `TabBarController`를 생성할 때에도 `NavigationController`를 한번 더 적용하여 이런 문제가 발생하는 것을 확인함.

`SceneDelegate.swift`
```swift
import UIKit

class SceneDelegate: UIResponder, UIWindowSceneDelegate {

    var window: UIWindow?

    func scene(_ scene: UIScene, willConnectTo session: UISceneSession, options connectionOptions: UIScene.ConnectionOptions) {
        guard let windowsScene = (scene as? UIWindowScene) else { return }
        
        window = UIWindow(windowScene: windowsScene)
        
        ...
        
        window?.rootViewController = UINavigationController(rootViewController: CustomTabBarController())
        window?.makeKeyAndVisible()
    }

}
```

`CustomTabBarController.swift`
```swift
import UIKit

final class CustomTabBarController: UITabBarController {
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        setupTabBar()
    }
    
    private func setupTabBar(){
        
        let bookListVC = UINavigationController(rootViewController: BookListViewController())
        
        viewControllers = [
            bookListVC
        ]
        
        ...
    }

}
```

## **해결**

아래와 같이 `NavigationController`로 감싸는 로직을 제거하고, `TabBarController`가 생성되어 `rootViewController`로 설정되게끔하여 해결함. 

`SceneDelegate.swift`
```swift
import UIKit

class SceneDelegate: UIResponder, UIWindowSceneDelegate {

    var window: UIWindow?

    func scene(_ scene: UIScene, willConnectTo session: UISceneSession, options connectionOptions: UIScene.ConnectionOptions) {
        guard let windowsScene = (scene as? UIWindowScene) else { return }
        
        window = UIWindow(windowScene: windowsScene)
        
        ...
        
        window?.rootViewController = CustomTabBarController()
        window?.makeKeyAndVisible()
    }

}
```

결과

![image](/assets/img/solveduplicatenavigationbar.gif)