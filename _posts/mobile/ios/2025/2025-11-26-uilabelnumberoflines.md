---
title: "UIKit - UILabel 긴 문장 표시하기"
date: 2025-11-26 17:51:00 +0900
categories: [Mobile, iOS]
tags: [uikit, code, uilabel]
---

`UILabel`은 정해진 사이즈 밖으로 글자가 나가면 ...으로 표시됨.

![image](/assets/img/notusenumberoflines.gif)

이런 문제를 `UILabel`의 `numberOfLines` 속성을 0으로 설정하여 여러 줄에 걸쳐 표시하도록 설정할 수 있음.

`TodoDetailView.swift`
```swift
import UIKit

class TodoDetailView: UIView {
    
    private let todoTitleLabel: UILabel = {
        let label = UILabel()
        
        label.font = UIFont.boldSystemFont(ofSize: 36)
        label.textColor = .white
        
        // 여러 줄에 걸쳐 텍스트 표시 설정
        label.numberOfLines = 0
        
        label.translatesAutoresizingMaskIntoConstraints = false
        
        return label
    }()
    

    ...

}
```

결과

![image](/assets/img/useuilabelnumberoflines.gif)

## **Reference**
- [https://ujeon.medium.com/ios-%EC%97%AC%EB%9F%AC-%EC%A4%84%EC%9D%98-%EB%AC%B8%EC%9E%A5%EC%9D%84-%EC%83%9D%EB%9E%B5%ED%95%98%EC%A7%80-%EC%95%8A%EA%B3%A0-%ED%91%9C%ED%98%84%ED%95%98%EA%B8%B0-639bc493c200](https://ujeon.medium.com/ios-%EC%97%AC%EB%9F%AC-%EC%A4%84%EC%9D%98-%EB%AC%B8%EC%9E%A5%EC%9D%84-%EC%83%9D%EB%9E%B5%ED%95%98%EC%A7%80-%EC%95%8A%EA%B3%A0-%ED%91%9C%ED%98%84%ED%95%98%EA%B8%B0-639bc493c200)