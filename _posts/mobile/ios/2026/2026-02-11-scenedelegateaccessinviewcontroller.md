---
title: "UIKit - ViewController에서 SceneDelegate 접근하기"
date: 2026-02-11 11:09:00 +0900
categories: [Mobile, iOS]
tags: [ios, uikit, viewcontroller, window, scenedelegate]
---

`ViewController` 내부에서 `SceneDelegate`에 접근하기 위해 아래와 같이 코드를 작성해서 처리함.

```swift
guard let sceneDelegate = self.view.window?.windowScene?.delegate as? SceneDelegate else { return }
```