---
title: "UIKit - UITextView 동적 높이 조정하기"
date: 2025-11-23 11:02:00 +0900
categories: [Mobile, iOS]
tags: [uikit, code, uitextview, delegate, height, cgsize]
---

`UITextView`에 높이를 동적으로 조정하기 위해선 아래와 같이 `Delegate` 메서드를 통해 구현할 수 있음.

`TodoAddViewController.swift`
```swift
// MARK: - TextView Delegate
extension TodoAddViewController: UITextViewDelegate{

    ...
    
    func textViewDidChange(_ textView: UITextView) {
        let size = CGSize(width: view.frame.width, height: .infinity)

        // textview의 적합한 사이즈로 계산
        let estimateSize = textView.sizeThatFits(size)
        
        textView.constraints.forEach { constraint in
            
            guard estimateSize.height > 150 && estimateSize.height < 250 else { return }
            
            if(constraint.firstAttribute == .height){
              // height constant 적용
                constraint.constant = estimateSize.height
            }
        }
    }
}
```

`TodoAddView.swift`
```swift
import UIKit

class TodoAddView: UIView {

    ...

    // MARK: - 할일 세부 사항(또는 메모) 텍스트 뷰
    let todoDescriptionTextView: UITextView = {
        let textView = UITextView()
        
        ....
        
        // isScrollEnabled false 설정
        textView.isScrollEnabled = false
        
        ....
        
        return textView
    }()

    ...
}
```

결과

![image](/assets/img/uitextviewdynamicheightresult.gif)

## **Reference**
- [https://velog.io/@hyesuuou/Swift-UITextView-%EB%86%92%EC%9D%B4%EB%A5%BC-Contents-height%EC%99%80-%EA%B0%99%EA%B2%8C-%ED%95%98%EA%B8%B0](https://velog.io/@hyesuuou/Swift-UITextView-%EB%86%92%EC%9D%B4%EB%A5%BC-Contents-height%EC%99%80-%EA%B0%99%EA%B2%8C-%ED%95%98%EA%B8%B0)
- [https://baked-corn.tistory.com/83](https://baked-corn.tistory.com/83)
- [https://liveupdate.tistory.com/461](https://liveupdate.tistory.com/461)