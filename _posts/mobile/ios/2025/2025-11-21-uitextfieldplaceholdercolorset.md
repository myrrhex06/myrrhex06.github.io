---
title: "UIKit - UITextField placeholder 색상 설정하기"
date: 2025-11-21 06:39:00 +0900
categories: [Mobile, iOS]
tags: [uikit, code, uitextfield, placeholder]
---

`UITextField` `placeholder`에 색상을 설정하는 것은 `NSAttributedString`을 사용해 처리 가능함.

`UITextField+PlaceholderColor.swift`
```swift
import UIKit

extension UITextField{
    
    func setPlaceholderColor(color: UIColor){
        guard let placeholder = self.placeholder else { return }
        attributedPlaceholder = NSAttributedString(string: placeholder, attributes: [NSAttributedString.Key.foregroundColor: color])
    }
}
```

`TodoAddView.swift`
```swift
import UIKit

class TodoAddView: UIView {
    
    // MARK: - 할일 제목 텍스트 필드
    private let todoTitleTextField: UITextField = {
        let textField = UITextField()
        
        textField.backgroundColor = UIColor(named: "textFieldColor")
        textField.placeholder = "What needs to be done?"

        // placeholder 색상 설정
        textField.setPlaceholderColor(color: .createdDate)
        
        textField.layer.borderColor = UIColor.gray.cgColor
        textField.layer.borderWidth = 0.3
        
        textField.textColor = .white
        textField.clipsToBounds = true
        textField.layer.cornerRadius = 8
        
        textField.translatesAutoresizingMaskIntoConstraints = false
        
        return textField
    }()
    
    // MARK: - 할일 세부 사항(또는 메모) 텍스트 필드
    private let todoDescriptionTextField: UITextField = {
        let textField = UITextField()
        
        textField.backgroundColor = UIColor(named: "textFieldColor")
        textField.placeholder = "Add details or notes..."
        
        // placeholder 색상 설정
        textField.setPlaceholderColor(color: .createdDate)
        
        textField.layer.borderColor = UIColor.gray.cgColor
        textField.layer.borderWidth = 0.3
        
        textField.textColor = .white
        textField.clipsToBounds = true
        textField.layer.cornerRadius = 8
        
        textField.translatesAutoresizingMaskIntoConstraints = false
        
        return textField
    }()

    ...
}
```

결과

![image](/assets/img/uitextfieldplaceholdercolorsetresult.gif)

## **Reference**
- [https://ios-development.tistory.com/369](https://ios-development.tistory.com/369)