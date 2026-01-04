---
title: "UIKit - UITextView 입력 글자 위치 조정하기"
date: 2025-11-22 20:40:00 +0900
categories: [Mobile, iOS]
tags: [uikit, code, uitextview, contentinset]
---

[이전글](https://myrrhex06.github.io/posts/textfieldpaddingapply/)에서 `UITextField`에 padding을 줘서 입력되는 문자의 위치를 잡는 방법에 대해 작성하였음.

`UITextView`는 `UITextField`처럼 `leftView`, `rightView`가 제공되지 않기 때문에 `textContainerInset`을 통해 여백을 조정해야함.

`textContainerInset`은 `textView` 내부의 상하좌우 `inset`을 설정할 수 있음.

> `contentInset`으로도 내부 상하좌우 `inset`을 조정할 수 있으나, 이것은 `UIScrollView`에서 사용하는 `inset`임.
{: .prompt-tip }

`TodoAddView.swift`
```swift
import UIKit

class TodoAddView: UIView {

    ...

    // MARK: - 할일 세부 사항(또는 메모) 텍스트 뷰
    let todoDescriptionTextView: UITextView = {
        let textView = UITextView()
        
        // textContainerInset 설정
        textView.textContainerInset = UIEdgeInsets(top: 15, left: 7, bottom: 15, right: 7)
        
        textView.backgroundColor = UIColor(named: "textFieldColor")
        
        textView.font = UIFont.systemFont(ofSize: 17)
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
}
```

결과

![image](/assets/img/textviewcontentinsetapplyresult.gif)

## **Reference**
- [https://gwangyonglee.tistory.com/55](https://gwangyonglee.tistory.com/55)
- [https://roniruny.tistory.com/149](https://roniruny.tistory.com/149)