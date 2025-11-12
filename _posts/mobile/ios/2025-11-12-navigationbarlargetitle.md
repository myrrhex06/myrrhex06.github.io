---
title: "UIKit - Navigation Large Title 설정하기"
date: 2025-11-11 17:52:00 +0900
categories: [Mobile, iOS]
tags: [uikit, code, navigationcontroller, navigationbar]
---

`LargeTitle` 설정에 필요한 코드
```swift
...

self.navigationController?.navigationBar.prefersLargeTitles = true
self.navigationItem.largeTitleDisplayMode = .always

...
```