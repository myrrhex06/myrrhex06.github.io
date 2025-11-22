---
title: "UIKit - UITextField 입력 글자 위치 조정하기"
date: 2025-11-22 17:53:00 +0900
categories: [Mobile, iOS]
tags: [uikit, code, uitextfield, placeholder, uiview, extension]
---

현재 할일 추가 화면을 `UITextField`, `UITextView`를 통해 구현함.

![image](/assets/img/beforeapplyingtextfieldpadding.gif)

`UITextField`에 입력되는 문자들의 위치가 너무 옆으로 붙어있어서 보기가 안좋음...

`UITextField`에 padding을 줘서 해결 가능함.

`UITextField+Padding.swift`
```swift
import UIKit

extension UITextField{
    
    func setPadding(){
        let paddingView = UIView(frame: CGRect(x: 0, y: 0, width: 10, height: 0))
        
        // 왼쪽 padding
        self.leftView = paddingView

        // 오른쪽 padding
        self.rightView = paddingView
        
        // placeholder, 입력 글자 모두 padding 설정
        self.leftViewMode = .always
        self.rightViewMode = .always
    }
}
```

> `UITextView`의 경우는 `UITextField`처럼 `leftView`, `rightView`가 없기 때문에 `contentInset`을 통해 설정해줘야함.
{: .prompt-tip }

결과

![image](/assets/img/afterapplytextfieldpadding.gif)

## **Reference**
- [https://dev-with-precious-dreams.tistory.com/142](https://dev-with-precious-dreams.tistory.com/142)
- [https://velog.io/@leeesangheee/TextField%EC%97%90-padding-%EB%84%A3%EB%8A%94-extension-%EB%A7%8C%EB%93%A4%EA%B8%B0](https://velog.io/@leeesangheee/TextField%EC%97%90-padding-%EB%84%A3%EB%8A%94-extension-%EB%A7%8C%EB%93%A4%EA%B8%B0)