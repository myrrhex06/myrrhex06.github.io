---
title: "UIKit - UITextView placeholder 커스텀하기"
date: 2026-04-14 20:18:00 +0900
categories: [Mobile, iOS]
tags: [ios, uikit, uitextview, delegate]
---

`UITextView`는 `UITextField`와는 달리 `placeholder`를 설정할 수 있는 프로퍼티가 제공되지 않음.

따라서, 직접 `delegate` 메서드를 통해 구현해줘야함.

`LargeInput.swift`
```swift
import UIKit

final class LargeInput: UIView {
    
    private var textView: UITextView = {
        let textView = UITextView()
        
        textView.textContainerInset = UIEdgeInsets(top: 10, left: 8, bottom: 10, right: 8)
        
        textView.backgroundColor = UIColor(named: "TextFieldBackgroundColor")
        textView.textColor = .white
        
        textView.autocapitalizationType = .none
        
        textView.autocorrectionType = .no
        textView.spellCheckingType = .no
        
        textView.layer.borderWidth = 1
        textView.layer.borderColor = UIColor(named: "BorderColor")?.cgColor
        
        textView.layer.cornerRadius = 8
        textView.clipsToBounds = true
        
        textView.font = UIFont.systemFont(ofSize: 17)
        
        return textView
    }()
    
    let textViewPlaceholder: UILabel = {
        let label = UILabel()
        
        label.text = "placeholder"
        label.textColor = .placeholder
        
        return label
    }()
    
    override init(frame: CGRect) {
        super.init(frame: frame)
        
        // placeholder label subview로 추가
        textView.addSubview(textViewPlaceholder)
        
        // 오토레이아웃 제약 설정
        textViewPlaceholder.snp.makeConstraints { make in
            make.top.equalToSuperview().inset(10)
            make.leading.equalToSuperview().inset(10)
        }
        
        ...
    }
    
    required init?(coder: NSCoder) {
        fatalError("init(coder:) has not been implemented")
    }
    
    func setLabelText(label: String){
        self.label.text = label
    }
    
    func setPlaceholder(placeholder: String?){
        self.textViewPlaceholder.text = placeholder
    }
    
    func setValidationText(text: String){
        self.validationLabel.text = text
    }
    
    func setDelegate(delegate: UITextViewDelegate){
        self.textView.delegate = delegate
    }
    
    func getTextView() -> UITextView{
        return textView
    }
    
    func setValidationAlpha(alpha: CGFloat){
        self.validationLabel.alpha = alpha
    }
}
```

위와 같이 `LargeInput`이라는 컴포넌트에서 `UITextView`에 `UILabel`을 사용하여 `placeholder`를 표시하도록 처리함.

`CreateBookViewController.swift`
```swift
extension CreateBookViewController: UITextViewDelegate{
    
    // 사용자가 text를 변경하기 전에 호출
    func textView(_ textView: UITextView, shouldChangeTextIn range: NSRange, replacementText text: String) -> Bool {

        // textView에 text가 비어있고, 새로 추가되는 text가 빈 값이 아닌 경우
        if textView.text.isEmpty && !text.isEmpty{
            
            // placeholder 제거
            switch textView{
            case createBookView.getContentTextView():
                createBookView.setContentPlaceholder(placeholder: nil)
            case createBookView.getFeelingTextView():
                createBookView.setFeelingPlaceholder(placeholder: nil)
            default:
                fatalError()
            }
        }
        
        // textView에 text 글자 수가 1보다 작거나 같고, 새로 넘어온 text가 빈 값이라면
        if textView.text.count <= 1 && text.isEmpty{
            
            // placeholder 설정
            switch textView{
            case createBookView.getContentTextView():
                createBookView
                    .setContentPlaceholder(
                        placeholder: Constants.CONTENT_PLACEHOLDER
                    )
            case createBookView.getFeelingTextView():
                createBookView
                    .setFeelingPlaceholder(
                        placeholder: Constants.FEELING_PLACEHOLDER
                    )
            default:
                fatalError()
            }
            
            return true
        }
        
        return true
    }   
}
```

위 코드와 같이 `delegate` 프로토콜을 상속하여 `placeholder` 렌더링 여부를 결정하는 로직을 작성함.

```swift
if textView.text.count <= 1 && text.isEmpty{
  ...
}
```

위 코드처럼 조건식을 0이 아니라 1보다 작거나 같다고 설정한 이유는 메서드의 실행 시점 때문임.

![image](/assets/img/invalidtextviewplaceholder.gif)

해당 메서드는 글자가 입력되기 전에 실행되기 때문에 위처럼 텍스트를 모두 지워도 `placeholder`가 나타나지 않음.

![image](/assets/img/invalidtextviewplaceholderlog.png)

로그를 확인해보면 위처럼 텍스트를 모두 지운다고 하더라도 해당 메서드가 실행되는 시점에서는 아직 백스페이스가 반영되기 전이기 때문에 `length` 값이 1로 남아있게됨.

이러한 이유로 1보다 작거나 같다는 조건을 사용해 `placeholder` 표시 여부를 결정하도록 로직을 구성함.

결과

![image](/assets/img/textviewplaceholderresult.gif)