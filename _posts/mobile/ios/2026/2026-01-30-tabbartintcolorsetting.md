---
title: "UIKit - TabBar 백그라운드 색상 설정하기"
date: 2026-01-30 09:39:00 +0900
categories: [Mobile, iOS]
tags: [ios, uikit, tabbar, color, tabbarcontroller]
---

기존에는 아래 코드처럼 `TabBar`의 백그라운드 색상을 적용했음.

```swift
self.tabBar.backgroundColor = UIColor(named: "BackgroundColor")
```

이렇게 색상을 적용하고 난 뒤 시뮬레이터를 구동시키면 아래와 같이 `TabBar`에 색상이 제대로 적용이 안된 것 같은 화면이 나타남.

![image](/assets/img/beforeapplytabbarcolor.gif)

이 문제를 해결하기 위해 아래와 같이 색상을 적용시키는 코드를 추가함.

```swift
self.tabBar.backgroundColor = UIColor(named: "BackgroundColor")
self.tabBar.barTintColor = .background
self.tabBar.isTranslucent = false
```

결과

![image](/assets/img/afterapplytabbarcolor.gif)

색상이 제대로 적용된 것을 확인할 수 있음.

## **Reference**
- [https://borabong.tistory.com/12#google_vignette](https://borabong.tistory.com/12#google_vignette)