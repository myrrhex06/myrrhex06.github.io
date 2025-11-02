---
title: "UIKit - 코드로 NavigationBar 버튼 추가하기"
date: 2025-11-02 16:30:00 +0900
categories: [Mobile, iOS]
tags: [uikit, code, navigationcontroller, navigationitem, navigationbar]
---

## **NavigationBar 버튼 추가 방법 (코드)**
`NavigationBar`에 버튼을 추가할 때는 `UIBarButtonItem` 를 사용해서 추가해야함.

**예시**

`SceneDelegate.swift`
```swift
import UIKit

class SceneDelegate: UIResponder, UIWindowSceneDelegate {

    var window: UIWindow?

    // NavigationController 루트 뷰 설정
    func scene(_ scene: UIScene, willConnectTo session: UISceneSession, options connectionOptions: UIScene.ConnectionOptions) {
        guard let windowScene = (scene as? UIWindowScene) else { return }
        
        window = UIWindow(windowScene: windowScene)
        
        let nav = UINavigationController(rootViewController: ViewController())
        
        window?.rootViewController = nav
        window?.makeKeyAndVisible()
    }

    ...
}
```

`ViewController.swift`
```swift
import UIKit

class ViewController: UIViewController {
    
    // 네비게이션 바 버튼 요소
    lazy var navigationBtn: UIBarButtonItem = {
        let item = UIBarButtonItem(barButtonSystemItem: .add, target: self, action: #selector(navigationBtnTapped))
        
        return item
    }()
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        view.backgroundColor = .white
        
        setNavigation()
    }

    func setNavigation(){
        title = "버튼 테스트"
        
        let appearance = UINavigationBarAppearance()
        
        appearance.configureWithOpaqueBackground()
        appearance.backgroundColor = .white
        
        navigationController?.navigationBar.tintColor = .blue
        navigationController?.navigationBar.standardAppearance = appearance
        navigationController?.navigationBar.scrollEdgeAppearance = appearance
        navigationController?.navigationBar.compactAppearance = appearance
        
        // 네비게이션 바 오른쪽에 버튼 추가
        navigationItem.rightBarButtonItem = navigationBtn
    }
    
    // 버튼 클릭 시 동작
    @objc func navigationBtnTapped(){
        let alert = UIAlertController(title: "테스트", message: "plus 버튼 테스트", preferredStyle: .alert)
        
        let okAction = UIAlertAction(title: "OK", style: .default, handler: nil)
        
        alert.addAction(okAction)
        
        present(alert, animated: true)
    }

}
```

**결과**

![image](/assets/img/navigationitemaddbycoderesult.gif)