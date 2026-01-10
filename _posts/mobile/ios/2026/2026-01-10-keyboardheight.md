---
title: "UIKit - NotificationCenter 키보드 height 구하기"
date: 2026-01-10 09:57:00 +0900
categories: [Mobile, iOS]
tags: [ios, uikit, notificationcenter, keyboard]
---

`NotificationCenter`를 통해 키보드가 올라왔을때, 혹은 내려갈때 시점을 처리할 수 있음.

이때 키보드의 `height`를 구하는 방법은 아래와 같음.

```swift
@objc func keyboardWillShow(notification: NSNotification){
        
  if let keyboardFrame = notification.userInfo?[UIResponder.keyboardFrameEndUserInfoKey] as? NSValue{
    let keyboardRectangle = keyboardFrame.cgRectValue
    let keyboardHeight = keyboardRectangle.height
  }
}
```

