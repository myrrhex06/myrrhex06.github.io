---
title: "UIKit - 키보드 수정 제안 탭 제거하기"
date: 2026-01-09 21:24:00 +0900
categories: [Mobile, iOS]
tags: [ios, uikit, uitextfield]
---

`UITextField`를 탭하여 키보드가 올라왔을 때 아래와 같이 수정 제안 탭이 표시됨.

![image](/assets/img/beforeremoveautocorrectionandspellingcheck.gif)

이것을 제거하기 위해서는 아래와 같이 코드를 구성하면 됨.

```swift
private let nicknameTextField: UITextField = {
  let textField = UITextField()
        
  ...
        
  textField.autocorrectionType = .no
  textField.spellCheckingType = .no
        
  ...
        
  return textField
}()
```

결과

![image](/assets/img/afterrmoveautocorrectionandspellingcheck.gif)

## **Reference**
- [https://jangsh9611.tistory.com/44](https://jangsh9611.tistory.com/44)