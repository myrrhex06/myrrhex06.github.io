---
title: "UIKit - UITextField 글자수 표시 및 제한하기"
date: 2025-12-01 21:34:00 +0900
categories: [Mobile, iOS]
tags: [uikit, code, uitextfield, delegate]
---

`UITextField`의 글자수를 표시 및 제한하는 기능은 `UITextField Delegate` 프로토콜 내에 있는 `textField(_ textField:, shouldChangeCharactersIn:, replacementString:)` 메서드를 통해 구현 가능함.

해당 메서드는 `UITextField`에 글자가 입력될 때마다 호출되며, `Bool` 타입을 반환값으로 가지고 있어 글자 입력 가능 여부를 결정할 수 있음.

`TodoAddViewController.swift`
```swift
import UIKit

final class TodoAddViewController: UIViewController {

    // MARK: - 할일 추가 화면
    let todoAddView = TodoAddView()

    ...

    func setupTodoAddView(){
        // TextField Delegate 설정
        todoAddView.setTextFieldDelegate(delegate: self)
    }

    ...
}


// MARK: - TextField Delegate
extension TodoAddViewController: UITextFieldDelegate{
    
    ...
    
    // UITextField 글자가 입력될 때 호출
    func textField(_ textField: UITextField, shouldChangeCharactersIn range: NSRange, replacementString string: String) -> Bool {
        
        guard let text = textField.text else { return false }
        
        // 문자 길이 계산
        let length = text.utf16.count + string.utf16.count - range.length
        
        // 글자수 표시
        todoAddView.setTodoCount(count: length)
        
        // 백스페이스는 예외처리
        if let char = string.cString(using: .utf8){
            let isBackspace = strcmp(char, "\\b")
            
            if isBackspace == -92 {
                return true
            }
        }
        
        // 입력 가능 수보다 크면 입력 불가능하도록 false 반환
        if length >= Constant.TODO_TITLE_MAX_LENGTH{
            return false
        }
        
        // 그 외의 경우는 입력 가능
        return true
    }
}
```

결과

![image](/assets/img/uitextfieldlengthlimitresult.gif)

## **Reference**

- [https://jiwift.tistory.com/entry/iOSSwift-UITextField-%EA%B8%80%EC%9E%90%EC%88%98-%EC%A0%9C%ED%95%9C-%EA%B8%80%EC%9E%90%EC%88%98-%EC%84%B8%EA%B8%B0](https://jiwift.tistory.com/entry/iOSSwift-UITextField-%EA%B8%80%EC%9E%90%EC%88%98-%EC%A0%9C%ED%95%9C-%EA%B8%80%EC%9E%90%EC%88%98-%EC%84%B8%EA%B8%B0)