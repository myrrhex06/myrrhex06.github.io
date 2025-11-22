---
title: "UIKit - UITextView 입력 글자 위치 조정하기"
date: 2025-11-22 20:02:00 +0900
categories: [Mobile, iOS]
tags: [uikit, code, uitextview, contentinset]
---

[이전글](https://myrrhex06.github.io/posts/textfieldpaddingapply/)에서 `UITextField`에 padding을 줘서 입력되는 문자의 위치를 잡는 방법에 대해 작성하였음.

`UITextView`는 `UITextField`처럼 `leftView`, `rightView`가 제공되지 않기 때문에 `contentInset`을 통해 여백을 조정해야함.

> `contentInset`은 하위뷰 요소의 여백을 주는 것임.
{: .prompt-tip }

`TodoAddView.swift`
```swift
import UIKit

class TodoAddView: UIView {

    ...

    // MARK: - 할일 세부 사항(또는 메모) 텍스트 뷰
    let todoDescriptionTextView: UITextView = {
        let textView = UITextView()
        
        // contentInset 설정
        textView.contentInset = UIEdgeInsets(top: 10, left: 10, bottom: 10, right: 10)
        
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