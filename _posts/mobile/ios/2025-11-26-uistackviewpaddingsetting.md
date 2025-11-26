---
title: "UIKit - UIStackView padding 설정하기"
date: 2025-11-26 21:00:00 +0900
categories: [Mobile, iOS]
tags: [uikit, code, uistackview, nsdirectionaledgeinsets, padding]
---

`UIStackView`에 `padding`을 설정하기 위해선 먼저 `isLayoutMarginsRelativeArrangement` 프로퍼티 값을 `true`로 설정해줘야함.

`isLayoutMarginsRelativeArrangement` 프로퍼티는 `margin` 사용 여부에 대한 값임.

> `true`로 설정하지 않을 경우 `padding` 값이 적용되지 않음.
{: .prompt-tip }

`UIStackView`의 `directionalLayoutMargins` 프로퍼티를 통해 `padding`을 설정할 수 있음.

> `directionalLayoutMargins` 프로퍼티는 `NSDirectionalEdgeInsets` 타입을 받는데, 이 타입은 `UIEdgeInsets`처럼 `left`, `right`가 아닌 `leading`, `trailing`을 받기 때문에 아랍권처럼 RTL(글 방향이 오른쪽 -> 왼쪽) 환경까지도 지원 가능함.
{: .prompt-tip }

`TodoDetailView.swift`
```swift
import UIKit

class TodoDetailView: UIView {

  ...

    private lazy var stackView: UIStackView = {
        let stackView = UIStackView(arrangedSubviews: [displayDescriptionLabel, todoDescriptionLabel])
        
        ....
        
        // isLayoutMarginsRelativeArrangement 프로퍼티 true 설정
        stackView.isLayoutMarginsRelativeArrangement = true

        // stackview padding 설정
        stackView.directionalLayoutMargins = NSDirectionalEdgeInsets(top: 15, leading: 15, bottom: 15, trailing: 15)
        
        ...
        
        return stackView
    }()

  ...
}
```

`padding` 적용 전 화면

![image](/assets/img/beforeapplyingstackviewpadding.gif)

`padding` 적용 후 화면

![image](/assets/img/afterapplyingstackviewpadding.gif)

## **Reference**
- [https://ios-development.tistory.com/1498](https://ios-development.tistory.com/1498)