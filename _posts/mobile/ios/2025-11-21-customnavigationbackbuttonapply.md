---
title: "UIKit - Navigation Back Button 타이틀 제거하기"
date: 2025-11-21 06:49:00 +0900
categories: [Mobile, iOS]
tags: [uikit, code, navigationcontroller, navigationitem, uibarbutton]
---

`UIBarButtonItem`의 타이틀을 빈 문자열로 설정해 `Navigation Back Button`의 텍스트를 제거할 수 있음.

> 예를 들어, A 화면에서 B 화면으로 push 되는 구조를 가졌다면, `UIBarButtonItem`은 A 화면 `ViewController`에서 설정해줘야함.
{: .prompt-tip }

`ViewController.swift`
```swift
import UIKit

final class ViewController: UIViewController {

    ...

    func setupNavigation(){
        self.navigationController?.navigationBar.prefersLargeTitles = true
        self.navigationItem.largeTitleDisplayMode = .always
        
        // UIBarButtonItem 생성 후 BackButton에 할당
        self.navigationItem.backBarButtonItem = UIBarButtonItem(title: "", style: .plain, target: self, action: nil)
        
        // 색상 white 설정
        self.navigationController?.navigationBar.tintColor = .white
        
        self.title = "Todolune"
    }

    ...
}
```

결과

![image](/assets/img/uibarbuttoncustombackbuttonapplyresult.gif)

## **Reference**
- [https://velog.io/@xanxnu/iOSSwift-Navigation-BackButton-%ED%83%80%EC%9D%B4%ED%8B%80-%EC%97%86%EC%95%A0%EA%B8%B0](https://velog.io/@xanxnu/iOSSwift-Navigation-BackButton-%ED%83%80%EC%9D%B4%ED%8B%80-%EC%97%86%EC%95%A0%EA%B8%B0)