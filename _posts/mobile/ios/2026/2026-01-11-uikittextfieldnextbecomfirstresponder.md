---
title: "UIKit - UITextField return 버튼 클릭 시 다음 요소로 넘어가도록 처리하기"
date: 2026-01-11 14:31:00 +0900
categories: [Mobile, iOS]
tags: [ios, uikit, uitextfield, delegate, return, button]
---

`UITextField`의 `delegate` 메서드를 통해서 `return` 버튼 클릭 시 다음 `UITextField` 요소로 넘어가도록 처리할 수 있음.

```swift
// MARK: - UITextField Delegate implement
extension RegisterViewController: UITextFieldDelegate{

  ...

  func textFieldShouldReturn(_ textField: UITextField) -> Bool {
        
    switch textField{
      case registerView.getEmailTextField():
        registerView.getPasswordTextField().becomeFirstResponder()
      case registerView.getPasswordTextField():
        registerView.getNicknameTextField().becomeFirstResponder()
      default:
        view.endEditing(true)
      }
        
      return true
  }
}
```

결과

![image](/assets/img/nextuitextfieldfirstresponder.gif)

## **Reference**
- [https://velog.io/@hyeonhee_bee/Swift-TIL68-textFieldShouldReturn-%ED%83%80%EC%9E%85%EC%97%90%EC%84%9C%EC%9D%98-Bool-%EB%A6%AC%ED%84%B4%EB%A6%AC%ED%84%B4%ED%82%A4-default-behavior-%EC%95%8C%EC%95%84%EB%B3%B4%EA%B8%B0](https://velog.io/@hyeonhee_bee/Swift-TIL68-textFieldShouldReturn-%ED%83%80%EC%9E%85%EC%97%90%EC%84%9C%EC%9D%98-Bool-%EB%A6%AC%ED%84%B4%EB%A6%AC%ED%84%B4%ED%82%A4-default-behavior-%EC%95%8C%EC%95%84%EB%B3%B4%EA%B8%B0)
- [https://leviblog.tistory.com/11](https://leviblog.tistory.com/11)
