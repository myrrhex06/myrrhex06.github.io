---
title: "UIKit - UITextField 여러 줄 입력 불가능한 이슈 UITextView를 통해 해결하기"
date: 2025-11-21 20:12:00 +0900
categories: [Mobile, iOS]
tags: [uikit, code, uitextfield, uitextview, delegate, placeholder]
---

`UITextField`를 통해 입력 필드를 구현한 후 테스트 도중 아래와 같은 이슈를 발견함...

![image](/assets/img/uitextfieldmultilineproblem.gif)

위 화면처럼 글자가 아래로 내려가지 않고 옆으로만 계속 작성되고 있음.

알아본 결과 `UITextField`는 여러 줄이 아닌 한 줄로만 입력을 받을 수 있다고 함.

따라서, `UITextField` 대신 `UITextView`를 통해 처리해줘야함.

`UITextField`와는 크게 다르지 않으나, `placeholder`를 제공해주지 않기 때문에 직접 `Delegate` 메서드를 구현해서 처리해줘야함.

`TodoAddView.swift`
```swift
import UIKit

class TodoAddView: UIView {

  ...

    // MARK: - 할일 세부 사항(또는 메모) 텍스트 뷰
    let todoDescriptionTextView: UITextView = {
        let textView = UITextView()
        
        textView.backgroundColor = UIColor(named: "textFieldColor")
        
        textView.font = UIFont.systemFont(ofSize: 17)

        // placeholder가 제공되지 않기 때문에 text에 설정 후 Delegate로 처리
        textView.text = Constant.DESCRIPTION_PLACEHOLDER
        textView.textColor = .createdDate
        
        textView.layer.borderColor = UIColor.gray.cgColor
        textView.layer.borderWidth = 0.3
        
        textView.clipsToBounds = true
        textView.layer.cornerRadius = 8
        
        textView.translatesAutoresizingMaskIntoConstraints = false
        
        return textView
    }()

  ...

    // MVC 패턴을 적용하여 View와 Controller가 분리되어 있기 때문에 Controller에서 넘겨줄 수 있도록 메서드를 만듬
    func setTextViewDelegate(delegate: UITextViewDelegate){
        todoDescriptionTextView.delegate = delegate
    }
}
```

`TodoAddViewController.swift`
```swift
import UIKit

class TodoAddViewController: UIViewController {

    // View 인스턴스 생성
    let todoAddView = TodoAddView()
    
    override func loadView() {
        // loadView 시점에 View 설정
        self.view = todoAddView
    }
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        // Delegate 설정
        todoAddView.setTextViewDelegate(delegate: self)
        setupNavigation()
    }

    ...
}

// MARK: - TextView Delegate
extension TodoAddViewController: UITextViewDelegate{

    // TextView가 focus를 얻는 경우 실행됨
    func textViewDidBeginEditing(_ textView: UITextView) {
        if textView.text == Constant.DESCRIPTION_PLACEHOLDER {
            textView.text = nil
            textView.textColor = .white
        }
    }
    
    // TextView가 focus를 잃는 경우 실행됨
    func textViewDidEndEditing(_ textView: UITextView) {
        if textView.text.trimmingCharacters(in: .whitespacesAndNewlines).isEmpty{
            textView.text = Constant.DESCRIPTION_PLACEHOLDER
            textView.textColor = .createdDate
        }
    }
}
```

결과

![image](/assets/img/uitextfieldmultilineresolve.gif)

## **Reference**
- [https://developer-fury.tistory.com/49](https://developer-fury.tistory.com/49)
- [https://ios-development.tistory.com/693](https://ios-development.tistory.com/693)
- [https://junbok97.tistory.com/246](https://junbok97.tistory.com/246)