---
title: "UIKit - UITextField 첫글자 대문자 변환 제거하기"
date: 2025-12-28 21:50:00 +0900
categories: [Mobile, iOS]
tags: [uikit, code, textfield]
---

`UITextField`에 영문 텍스트를 입력하면 첫글자가 대문자로 자동 변환됨.

이 설정을 제거하기 위해선 `autocapitalizationType` 프로퍼티를 `none` 설정해줘야함.

```swift
import UIKit
import SnapKit

final class RegisterView: UIView {
    
    private let emailTextField: UITextField = {
        let textField = UITextField()
        
        ...
        
        // 첫글자 대문자 변환 설정 제거
        textField.autocapitalizationType = .none
        
        ...
        
        return textField
    }()

}
```