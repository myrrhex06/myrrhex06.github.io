---
title: "UIKit - UITextView 글자수 제한하기"
date: 2025-11-27 21:57:00 +0900
categories: [Mobile, iOS]
tags: [uikit, code, uitextview, delegate]
---

사이드 프로젝트로 진행하던 할일 관리 앱의 할일 추가 화면을 기존에는 사용자가 입력하는 글자가 `UITextView` `height` 크기를 넘어가면 동적으로 늘어나도록 설정하였음.

이렇게 구현하니까 동작 자체가 너무 부자연스럽고 거기다 글자를 계속 입력하면 `view` 아래로 넘어가는 현상까지 발생함.

`Delegate` 메서드를 통해 제어하거나 스크롤뷰를 적용할까도 생각해봤지만 
불필요하게 입력 영역이 커지는 것을 방지하고, 레이아웃을 좀 더 안정적으로 잡기 위해서 `UITextView`의 `height` 크기를 고정하고 글자수를 제한하는 것으로 결정함.

글자수 제한 기능도 `Delegate` 메서드 내에서 구현 가능함.

`TodoAddViewController.swift`
```swift
// MARK: - TextView Delegate
extension TodoAddViewController: UITextViewDelegate{

    ...
    
    func textViewDidChange(_ textView: UITextView) {
        
        // UITextView에 입력된 텍스트 가져오기
        guard let text = textView.text else { return }
        
        // 사이즈 추출
        let count = text.count
        
        // 상수로 설정해둔 최대 입력 가능 글자 크기보다 클 경우
        if count > Constant.TODO_DESCRIPTION_MAX_LENGTH {
            
            // 최대 크기에 맞게 문자열 자르기
            let prefix = String(text.prefix(Constant.TODO_DESCRIPTION_MAX_LENGTH))
            
            // UITextView에 재할당
            textView.text = prefix
        }
        
        todoAddView.setDescriptionCount(count: textView.text.count)
    }
}
```

결과

![image](/assets/img/uitextviewconstraintcountresult.gif)