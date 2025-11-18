---
title: "UIKit - Navigation Title 색상 설정"
date: 2025-11-19 06:39:00 +0900
categories: [Mobile, iOS]
tags: [uikit, code, navigatino, title, uicolor]
---

`SceneDelegate.swift`
```swift
import UIKit

class SceneDelegate: UIResponder, UIWindowSceneDelegate {

    var window: UIWindow?

    func scene(_ scene: UIScene, willConnectTo session: UISceneSession, options connectionOptions: UIScene.ConnectionOptions) {
        guard let windowScene = (scene as? UIWindowScene) else { return }
        
        window = UIWindow(windowScene: windowScene)
        
        let nav = UINavigationController(rootViewController: ViewController())
        
        let appearance = UINavigationBarAppearance()
        
        appearance.configureWithOpaqueBackground()
        appearance.backgroundColor = UIColor(named: "backgroundColor")

        // Large Title 색상 설정
        appearance.largeTitleTextAttributes = [.foregroundColor: UIColor.white]

        // Title 색상 설정
        appearance.titleTextAttributes = [.foregroundColor: UIColor.white]
        
        UINavigationBar.appearance().scrollEdgeAppearance = appearance
        UINavigationBar.appearance().standardAppearance = appearance
        UINavigationBar.appearance().compactAppearance = appearance
        
        window?.rootViewController = nav
        window?.makeKeyAndVisible()
    }

    ...
}
```