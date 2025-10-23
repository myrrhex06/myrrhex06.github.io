---
title: "UIKit - 버튼 이벤트 핸들러 설정 방법"
date: 2025-10-21 06:50:00 +0900
categories: [Mobile, iOS]
tags: [uikit, code, uibutton, addtarget, event]
---

## **버튼 이벤트 핸들러 설정 방법**
`UIButton`에 내장되어 있는 `addTarget` 메서드를 통해 처리 가능함.

`addTarget` 메서드 Document

![image](/assets/img/addTargetDocument.png)

**메서드 매개 변수**
- `target`: 누가 처리할 것인지
- `action`: 이벤트가 발생하면 어떤 메서드를 호출할 것인지
- `controlEvents`: 어떤 이벤트를 핸들링할 것인지

예시
```swift
import UIKit

class ViewController: UIViewController {
    
    private lazy var exBtn: UIButton = {
        let btn = UIButton()
        
        btn.setTitle("이벤트", for: .normal)
        btn.backgroundColor = .black
        btn.setTitleColor(.white, for: .normal)

        // 이벤트 처리 설정
        btn.addTarget(self, action: #selector(ViewController.handleEvent), for: .touchUpInside)
        
        return btn
    }()

    override func viewDidLoad() {
        super.viewDidLoad()
        
        // 하위 뷰 추가
        self.view.addSubview(exBtn)
        
        // 자동 프레임 조정 해제
        exBtn.translatesAutoresizingMaskIntoConstraints = false
        
        // 오토 레이아웃 제약 설정
        exBtn.leadingAnchor.constraint(equalTo: self.view.leadingAnchor, constant: 30).isActive = true
        exBtn.trailingAnchor.constraint(equalTo: self.view.trailingAnchor, constant: -30).isActive = true
        exBtn.centerYAnchor.constraint(equalTo: self.view.centerYAnchor).isActive = true
    }
    
    @objc func handleEvent(){
        print("버튼이 눌렸음")
    }
}
```

> `lazy var`를 사용해서 지연 초기화를 해주는 이유는 저장 프로퍼티가 `self`보다 먼저 초기화 되기 떄문임. 실행에는 문제가 없으나 경고창이 뜨는게 불편하다면 `lazy var`를 통해 처리해야함.
{: .prompt-tip }


위 예시에서 주의깊게 봐야할 부분은 아래 코드임.

```swift
// 이벤트 처리 설정
btn.addTarget(self, action: #selector(ViewController.handleEvent), for: .touchUpInside)
```

- `target`: `self` -> 자기 자신(위 코드에선 `ViewController`)이 처리함.
- `action`: `#selector(ViewController.handleEvent)` -> 특정 이벤트가 발생했을 때 `handleEvent`를 호출함
- `for`: `touchUpInside` -> `touchUpInside` 이벤트를 핸들링

