---
title: "UIKit - UILabel 텍스트 left-top 정렬하기"
date: 2025-11-28 20:15:00 +0900
categories: [Mobile, iOS]
tags: [uikit, code, uilabel]
---

사이드 프로젝트를 개발하던 중, `UILabel`에 텍스트를 left-top 정렬을 해야하는 상황이 생겼다.

![image](/assets/img/beforeapplylefttopalignment.gif)

위 화면과 같이 텍스트가 부자연스럽게 정렬되어 있는 상황....

이런 문제를 해결하기 위해 구글링을 통해 알아낸 결과 아래와 같은 커스텀뷰를 만들어 해결할 수 있다는 것을 알아냈다.

`UIVerticalAlignLabel.swift`
```swift
import UIKit

class UIVerticalAlignLabel: UILabel {

    enum VerticalAlignment {
        case top
        case middle
        case bottom
    }

    var verticalAlignment : VerticalAlignment = .top {
        didSet {
            setNeedsDisplay()
        }
    }

    override public func textRect(forBounds bounds: CGRect, limitedToNumberOfLines: Int) -> CGRect {
        let rect = super.textRect(forBounds: bounds, limitedToNumberOfLines: limitedToNumberOfLines)

        if UIView.userInterfaceLayoutDirection(for: .unspecified) == .rightToLeft {
            switch verticalAlignment {
            case .top:
                return CGRect(x: self.bounds.size.width - rect.size.width, y: bounds.origin.y, width: rect.size.width, height: rect.size.height)
            case .middle:
                return CGRect(x: self.bounds.size.width - rect.size.width, y: bounds.origin.y + (bounds.size.height - rect.size.height) / 2, width: rect.size.width, height: rect.size.height)
            case .bottom:
                return CGRect(x: self.bounds.size.width - rect.size.width, y: bounds.origin.y + (bounds.size.height - rect.size.height), width: rect.size.width, height: rect.size.height)
            }
        } else {
            switch verticalAlignment {
            case .top:
                return CGRect(x: bounds.origin.x, y: bounds.origin.y, width: rect.size.width, height: rect.size.height)
            case .middle:
                return CGRect(x: bounds.origin.x, y: bounds.origin.y + (bounds.size.height - rect.size.height) / 2, width: rect.size.width, height: rect.size.height)
            case .bottom:
                return CGRect(x: bounds.origin.x, y: bounds.origin.y + (bounds.size.height - rect.size.height), width: rect.size.width, height: rect.size.height)
            }
        }
    }

    override public func drawText(in rect: CGRect) {
        let r = self.textRect(forBounds: rect, limitedToNumberOfLines: self.numberOfLines)
        super.drawText(in: r)
    }

}
```

결과

![image](/assets/img/afterapplylefttopalignment.gif)

## **Reference**
- [https://youjean.tistory.com/19](https://youjean.tistory.com/19)