---
title: "UIKit - 점선(Dash border) 적용하기"
date: 2026-04-09 06:38:00 +0900
categories: [Mobile, iOS]
tags: [ios, uikit, cashapelayer, cgmutablepath]
---

UIKit에서는 점선을 그려면 `CAShapeLayer` 클래스를 사용해서 직접 점선을 그려줘야함.

`CreateBookViewController.swift`
```swift
import UIKit

final class CreateBookViewController: UIViewController {

  private let createBookView = CreateBookView()

  override func viewDidLayoutSubviews() {
    super.viewDidLayoutSubviews()

    // 레이어가 중첩되지 않도록 제거
    createBookView.chooseBookImageContainer.layer.sublayers?.removeAll(where: { $0 is CAShapeLayer})

    // 패턴 정의
    let lineDashPattern: [NSNumber]? = [15, 3]

    let shapeLayer = CAShapeLayer()

    // 색상 지정
    shapeLayer.strokeColor = UIColor.border.cgColor

    // 두께 지정
    shapeLayer.lineWidth = 4

    // 패턴 지정
    shapeLayer.lineDashPattern = lineDashPattern

    // 선 그리기
    let path = UIBezierPath(rect: createBookView.chooseBookImageContainer.bounds)

    shapeLayer.path = path.cgPath
    shapeLayer.fillColor = .none

    createBookView.chooseBookImageContainer.layer.addSublayer(shapeLayer)
  }
}
```

`view`의 정확한 `width`, `height`를 사용하기 위해서 `viewDidLayoutSubviews` 내부에서 처리함.

결과

![image](/assets/img/dashborderapplyresult.gif)

## **Reference**
- [https://taekki-dev.tistory.com/106](https://taekki-dev.tistory.com/106)
- [https://velog.io/@emilyj4482/SpriteKit-%EA%B3%B5-%EA%B5%B4%EB%A6%AC%EA%B8%B0-%EA%B2%8C%EC%9E%84-%EB%A7%8C%EB%93%A4%EA%B8%B0-chapter.01](https://velog.io/@emilyj4482/SpriteKit-%EA%B3%B5-%EA%B5%B4%EB%A6%AC%EA%B8%B0-%EA%B2%8C%EC%9E%84-%EB%A7%8C%EB%93%A4%EA%B8%B0-chapter.01)