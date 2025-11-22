---
title: "UIKit - UITextField placeholder 커스텀하기"
date: 2025-11-22 12:10:00 +0900
categories: [Mobile, iOS]
tags: [uikit, code, uitextfield, placeholder]
---

`UITextField`, `UITextView`를 모두 사용하여 할 일 추가 화면을 구현하였는데, `UITextView`에는 `placeholder`가 제공되지 않아 `Delegate`를 통해 직접 구현함.

이렇게 구현하니까 아래 화면처럼 `UITextField`와 `UITextView` `placeholder`가 서로 다르게 동작하게 됨.

![image](/assets/img/textfieldtextviewplaceholdermismatches.gif)

그래서 `UITextField`도 `Delegate` 메서드를 구현해서 `placeholder`를 구현해주기로 함.

`TodoAddView.swift`
```swift
import UIKit

class TodoAddView: UIView {

    // MARK: - 할일 제목 텍스트 필드
    private let todoTitleTextField: UITextField = {
        let textField = UITextField()
        
        textField.backgroundColor = UIColor(named: "textFieldColor")

        // placeholder를 사용하지 않고 text 프로퍼티에 placeholder 값 할당
        textField.text = Constant.TITLE_PLACEHOLDER
        textField.textColor = .createdDate
        
        textField.font = UIFont.systemFont(ofSize: 17)
        
        textField.layer.borderColor = UIColor.gray.cgColor
        textField.layer.borderWidth = 0.3
        
        textField.clipsToBounds = true
        textField.layer.cornerRadius = 8
        
        textField.translatesAutoresizingMaskIntoConstraints = false
        
        return textField
    }()

    ...


    // Controller에서 View에 있는 UI 요소에 접근하지 못하기 때문에 Delegate를 설정하는 메서드를 만들어줌
    func setTextFieldDelegate(delegate: UITextFieldDelegate){
        todoTitleTextField.delegate = delegate
    }
}
```

`TodoAddViewController.swift`
```swift
import UIKit

class TodoAddViewController: UIViewController {

    let todoAddView = TodoAddView()
    
    override func loadView() {
        self.view = todoAddView
    }
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        // viewDidLoad 시점에서 Delegate 설정
        todoAddView.setTextFieldDelegate(delegate: self)
        setupNavigation()
    }

    ...
}

// MARK: - TextField Delegate
extension TodoAddViewController: UITextFieldDelegate{
    
    // TextField 입력이 시작되는 시점에 호출
    func textFieldDidBeginEditing(_ textField: UITextField) {
        if textField.text == Constant.TITLE_PLACEHOLDER {
            textField.text = nil
            textField.textColor = .white
        }
    }
    
    // TextField 입력이 끝나는 시점에 호출
    func textFieldDidEndEditing(_ textField: UITextField) {
        guard let text = textField.text, !text.isEmpty else {
            textField.text = Constant.TITLE_PLACEHOLDER
            textField.textColor = .createdDate
            
            return
        }
    }
}
```

결과

![image](/assets/img/textfieldtextviewplaceholdermismatchesresolve.gif)


## **Reference**
- [https://velog.io/@delmasong/UITextFieldDelegate](https://velog.io/@delmasong/UITextFieldDelegate)
- [https://ios-daniel-yang.tistory.com/entry/UITextField](https://ios-daniel-yang.tistory.com/entry/UITextField)