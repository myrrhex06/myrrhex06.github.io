---
title: "UIKit - Symbol Image 사이즈 설정하기"
date: 2026-02-25 17:49:00 +0900
categories: [Mobile, iOS]
tags: [ios, uikit, uiimage, uiimageview, image, configuration]
---

예시
```swift
private let contentTextDisplayImageView: UIImageView = {
  let imageView = UIImageView()
          
  let configuration = UIImage.SymbolConfiguration(pointSize: 17)
  imageView.image = UIImage(systemName: "book.pages", withConfiguration: configuration)

  return imageView
}()
```