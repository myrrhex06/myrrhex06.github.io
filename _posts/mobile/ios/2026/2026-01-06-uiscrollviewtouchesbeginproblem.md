---
title: "UIKit - UIScrollView touchesBegan 메서드가 호출되지 않는 문제 해결하기"
date: 2026-01-06 21:25:00 +0900
categories: [Mobile, iOS]
tags: [ios, uikit, uiscrollview, touch, gesture]
---

`UIScrollView`를 적용하여 `View`를 구성하였고, `ViewController`에서 `touchesBegan` 메서드를 오버라이딩하여 화면을 터치했을 때 키보드가 내려가도록 처리하였음.

`touchesBegan` 구현 내용
```swift
import UIKit

final class RegisterViewController: UIViewController {

    ...

    override func touchesBegan(_ touches: Set<UITouch>, with event: UIEvent?) {
        self.view.endEditing(true)
    }

    ...

}
```

하지만 아래 사진처럼 키보드가 올라온 상황에서 아무 화면이나 터치했을 때 `touchesBegan`이 제대로 실행되지 않고있음.

![image](/assets/img/uiscrollviewtouchesbeginproblem.gif)

이 동작의 원인은 `UIScrollView`가 터치 이벤트를 스크롤 동작으로 더 먼저 처리하기 때문임.

이를 해결하기 위해선 `UITapGestureRecognizer`를 통해서 제스처를 처리해줘야함.

`RegisterView.swift`
```swift
import UIKit
import SnapKit

final class RegisterView: UIView {

    func setupTapGesture(tapGesture: UITapGestureRecognizer){

        // UITapGestureRecognizer를 scrollView에 설정
        scrollView.addGestureRecognizer(tapGesture)
    }
}
```

`RegisterViewController.swift`
```swift
import UIKit

final class RegisterViewController: UIViewController {
    
    private let registerView = RegisterView()
    
    override func loadView() {
        
        ...

        // UITapGestureRecognizer 인스턴스 생성 
        let gesture = UITapGestureRecognizer(target: self, action: #selector(tapGesture))

        // UITapGestureRecognizer가 터치 이벤트를 처리한 후 다른 view에도 전파되도록 설정
        gesture.cancelsTouchesInView = false

        // scrollView에 제스처 적용
        registerView.setupTapGesture(tapGesture: gesture)
        
        self.view = registerView
    }
    
    @objc private func tapGesture(){

        // 키보드 내리기
        view.endEditing(true)
    }

    ...
}
```

`cancelsTouchesInView` 프로퍼티는 `UITapGestureRecognizer`가 터치를 인식한 후 해당 터치 이벤트를 하위 `view`에 전달하지 않고 취소할지 여부를 결정함. 

> `cancelsTouchesInView` 프로퍼티의 기본값은 `true` 이며, 이 경우엔 제스처가 인식되면 하위 `view`의 터치 이벤트(`touchesBegan`, `touchUpInside` 등)는 취소되어 전달되지 않게됨.
{: .prompt-tip }

결과

![image](/assets/img/uiscrollviewtouchesbeginresolve.gif)

## **Reference**
- [https://until.blog/@meowbutlerdev/-uikit--scrollview%EC%97%90%EC%84%9C-touchesbegan%EC%9D%B4-%ED%98%B8%EC%B6%9C%EB%90%98%EC%A7%80-%EC%95%8A%EB%8A%94-%EB%AC%B8%EC%A0%9C-%ED%95%B4%EA%B2%B0%ED%95%98%EA%B8%B0](https://until.blog/@meowbutlerdev/-uikit--scrollview%EC%97%90%EC%84%9C-touchesbegan%EC%9D%B4-%ED%98%B8%EC%B6%9C%EB%90%98%EC%A7%80-%EC%95%8A%EB%8A%94-%EB%AC%B8%EC%A0%9C-%ED%95%B4%EA%B2%B0%ED%95%98%EA%B8%B0)