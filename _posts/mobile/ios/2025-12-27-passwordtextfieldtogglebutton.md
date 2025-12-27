---
title: "UIKit - UITextField 비밀번호 마스킹 토글 버튼 구현"
date: 2025-12-27 11:20:00 +0900
categories: [Mobile, iOS]
tags: [uikit, code, textfield, component]
---

비밀번호 입력 `TextField`에 토근 버튼을 넣기 위해 아래와 같이 구현함.

`UITexTield`의 `rightView` 속성을 통해 처리하였음.

`PasswordTextField.swift`
```swift
import UIKit

final class PasswordTextField: UITextField {

    private var rightButton: UIButton!
    
    override init(frame: CGRect) {
        super.init(frame: frame)
        
        setupUI()
    }
    
    required init?(coder: NSCoder) {
        fatalError()
    }

    private func setupUI(){
        // 비밀번호 마스킹 처리
        isSecureTextEntry = true
        textContentType = .oneTimeCode
        
        // 왼쪽 패딩
        let leftPaddingView = UIView(frame: CGRect(x: 0, y: 0, width: 10, height: 0))
        
        // 오른쪽 패딩
        let rightPaddingView = UIView(frame: CGRect(x: 0, y: 0, width: bounds.height - 40, height: bounds.height - 30))
        
        // 토글 버튼 생성
        rightButton = UIButton(frame: CGRect(x: -5, y: 0, width: bounds.height - 30, height: bounds.height - 30))
        rightButton.contentMode = .scaleAspectFit
        rightButton.setImage(UIImage(systemName: "eye.slash.fill"), for: .normal)
        
        rightPaddingView.addSubview(rightButton)
        
        // UITextField rightView 설정
        rightView = rightPaddingView
        rightViewMode = .always

        // UITextField leftView 설정
        leftView = leftPaddingView
        leftViewMode = .always
        
        rightButton.tintColor = UIColor(named: "PlaceholderColor")
        rightButton.addTarget(self, action: #selector(updateStatus), for: .touchUpInside)
    }
    
    @objc private func updateStatus(){
        isSecureTextEntry.toggle()
        if isSecureTextEntry{
            rightButton.setImage(UIImage(systemName: "eye.slash.fill"), for: .normal)
        }else{
            rightButton.setImage(UIImage(systemName: "eye.fill"), for: .normal)
        }
    }
}
```

결과

![image](/assets/img/passwordtextfieldtogglebutton.gif)

## **Reference**
- [https://velog.io/@comdongsam/UITextField%EC%97%90-%EB%B2%84%ED%8A%BC-%EB%84%A3%EA%B8%B0-feat.-rightView-bpk5f3m7](https://velog.io/@comdongsam/UITextField%EC%97%90-%EB%B2%84%ED%8A%BC-%EB%84%A3%EA%B8%B0-feat.-rightView-bpk5f3m7)
- [https://ios-development.tistory.com/286](https://ios-development.tistory.com/286)