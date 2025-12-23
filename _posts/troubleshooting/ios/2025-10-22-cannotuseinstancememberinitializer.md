---
title: "Cannot use instance member 'exBtn1' within property initializer; property initializers run before 'self' is available 해결"
date: 2025-10-22 20:30:00 +0900
categories: [Troubleshooting]
tags: [uikit, stackview]
---

## **에러**
`UIStackView` 사용 방법에 대해 연습하던 도중 아래와 같은 에러가 발생함.

발생 에러
![image](/assets/img/cannotuseinstancemember.png)

## **원인**
전체 코드를 보면 아래와 같음.
```swift
import UIKit

final class ViewController: UIViewController {
    
    private let exBtn1: UIButton = {
        let btn = UIButton()
        
        btn.setTitle("exBtn1", for: .normal)
        btn.backgroundColor = .black
        btn.setTitleColor(.white, for: .normal)
        
        return btn
    }()
    
    private let exBtn2: UIButton = {
        let btn = UIButton()
        
        btn.setTitle("exBtn2", for: .normal)
        btn.backgroundColor = .black
        btn.setTitleColor(.white, for: .normal)
        
        return btn
    }()
    
    private let exBtn3: UIButton = {
        let btn = UIButton()
        
        btn.setTitle("exBtn3", for: .normal)
        btn.backgroundColor = .black
        btn.setTitleColor(.white, for: .normal)
        
        return btn
    }()
    
    private let exStackView: UIStackView = {
        let stackView = UIStackView(arrangedSubviews: [exBtn1, exBtn2, exBtn3])
        
        stackView.axis = .vertical
        stackView.spacing = 10
        stackView.alignment = .center
        stackView.distribution = .fillEqually
        
        return stackView
    }()

    ...
}
```

현재 `UIStackView`의 생성자를 통해 `UIStackView`의 인스턴스를 생성하려고 하지만, 여기에는 큰 문제가 있음.

```swift
...
private let exStackView: UIStackView = {
    let stackView = UIStackView(arrangedSubviews: [exBtn1, exBtn2, exBtn3])
}
...
```

위 코드처럼 같은 클래스 내에 정의된 프로퍼티를 사용하여 생성자로 인스턴스를 만들려고 할 경우 `self` 키워드 보다 저장 프로퍼티가 먼저 초기화 되기 때문에 타이밍이 맞지 않는 문제가 발생하게 됨.

쉽게 말하면, 아직 `self`가 완전히 만들어지기 전에 `self.exBtn1` 같은 내부 프로퍼티에 접근하려다 실패하는 것임.


## **해결**
`lazy var`를 통한 지연 초기화를 사용하면 실제로 프로퍼티에 접근할 때 클로저가 실행되기 때문에 `self`가 완전히 초기화되는 시점을 맞출 수 있음.

이를 통해 발생한 에러를 해결 가능함.

예시
```swift
import UIKit

final class ViewController: UIViewController {
  ...

  // lazy var를 통한 지연 초기화
  private lazy var exStackView: UIStackView = {
      let stackView = UIStackView(arrangedSubviews: [exBtn1, exBtn2, exBtn3])
        
      stackView.axis = .vertical
      stackView.spacing = 10
      stackView.alignment = .center
      stackView.distribution = .fillEqually
        
      return stackView
  }()

  ...
}
```