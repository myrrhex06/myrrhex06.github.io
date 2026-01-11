---
title: "UIKit - 기준 해상도를 활용한 기기별 UI 비율 처리하기"
date: 2026-01-11 12:17:00 +0900
categories: [Mobile, iOS]
tags: [ios, uikit, uiscreen, cgfloat]
---

iPhone은 기기별로 화면 해상도와 비율이 서로 다름.

지난 사이드 프로젝트를 개발할 때에는 화면이 얼마 되지 않아 일일이 기기별로 분기를 해줬었음.

또 다른 방법으로는 오토 레이아웃 제약을 설정할 때 비율로 값을 줘서 기기별로 제약 조건을 설정할 수 있음.

내 경우에는 iPhone 16 Pro 해상도를 기준으로 처리함.

```swift
import UIKit

extension CGFloat{
    
    /// 기기별 width 비율
    var deviceWidth: CGFloat{
        let ratio = UIScreen.main.bounds.width / 402
        return self * ratio
    }
    
    /// 기기별 height 비율
    var deviceHeight: CGFloat{
        let ratio = UIScreen.main.bounds.height / 874
        return self * ratio
    }
}
```

사용 예시
```swift
import UIKit
import SnapKit

final class RegisterView: UIView {

  ...

  private func setupRegisterButton(){
    contentView.addSubview(registerButton)
        
    registerButton.snp.makeConstraints { make in
      make.top.equalTo(stackView.snp.bottom).offset(CGFloat(95).deviceHeight)
      make.bottom.equalTo(contentView.safeAreaLayoutGuide.snp.bottom).inset(20)
      make.leading.equalToSuperview().inset(20)
      make.trailing.equalToSuperview().inset(20)
      make.height.equalTo(60)
    }
  }

  ...

}
```

## **Reference**
- [https://github.com/jane1choi/TIL/issues/1](https://github.com/jane1choi/TIL/issues/1)