---
title: "@IBAction, @IBOutlet이란?"
date: 2025-10-14 21:45:00 +0900
categories: [Mobile, iOS]
tags: [uikit, storyboard, ibaction, iboutlet, label, button]
---

## **@IBAction이란?**
- 버튼 같은 UI 요소가 액션을 일으킬 때 실행되는 함수에 붙이는 어노테이션.
  - 버튼을 클릭했을 때, 드래그 했을 때 이런 동작들을 액션이라고 함.

요소를 선택한 후 `Control` 키를 누른 상태에서 코드 영역으로 드래그하면 아래와 같은 화면이 나타남.

![image](/assets/img/btnactionconnecttap.png)

나타난 탭에서 연결되는 함수명(Name), 이벤트(Event), 타입(Type) 설정이 가능함.

이벤트의 경우 아래와 같이 다양한 종류의 이벤트가 제공됨.

![image](/assets/img/actionevents.png)

> 대부분의 경우 `Touch Up Inside`를 사용하며, 해당 이벤트는 버튼을 눌렀다가 뗐을 때를 뜻함.
{: .prompt-tip }

모든 설정이 끝난 후 `Connect`를 누르면 아래와 같이 스토리보드 요소의 이벤트를 처리하는 역할을 하는 메서드가 생성됨.

```swift
import UIKit

class ViewController: UIViewController {
    
    override func viewDidLoad() {
        super.viewDidLoad()
    }
    
    @IBAction func exBtnAction(_ sender: Any) {
    }
}
```

해당 메서드에 설정된 이벤트가 발생했을 경우 처리할 로직을 작성하여 이벤트를 처리할 수 있음.

예시
```swift
import UIKit

class ViewController: UIViewController {
    
    override func viewDidLoad() {
        super.viewDidLoad()
    }
    
    @IBAction func exBtnAction(_ sender: Any) {
	    print("Hello World")
    }
}
```

위 예시의 경우 버튼이 클릭되었을 때 콘솔에 `Hello World` 문자열이 출력됨.

## **@IBOutlet이란?**
- 스토리보드 상에 있는 UI 요소를 코드에서 직접 조작할 수 있도록 연결하는 어노테이션.

`@IBAction`과 마찬가지로 스토리보드에 요소를 선택한 후 `Control` 단축키를 누른 상태에서 코드 영역으로 드래그하면 아래와 같은 탭이 나타남.

![image](/assets/img/labelconnecttap.png)

`@IBAction`에서 나타난 탭과는 살짝 다른 것을 알 수 있음.

해당 탭에서는 변수명(Name)을 설정한 후 `Connect`를 누르면 아래 코드와 같이 변수에 스토리보드 요소가 연결됨.

```swift
import UIKit

class ViewController: UIViewController {

    @IBOutlet weak var exLabel: UILabel!
    
    override func viewDidLoad() {
        super.viewDidLoad()
    }
}
```

해당 요소를 조작하여 요소의 텍스트나 색깔 등을 변경 가능함.

예시
```swift
import UIKit

class ViewController: UIViewController {

    @IBOutlet weak var exLabel: UILabel!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        exLabel.text = "Hello World!"
    }
}
```

`viewDidLoad()` 메서드 내부에서 `Label`의 `text` 속성을 변경한 것을 확인할 수 있음.

> `viewDidLoad()` 메서드는 앱 화면이 메모리에 처음 로드되는 시점에 한번만 호출됨.
{: .prompt-tip }