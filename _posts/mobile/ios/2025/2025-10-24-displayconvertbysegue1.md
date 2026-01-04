---
title: "UIKit - 화면 전환 및 데이터 전달(간접 세그웨이)"
date: 2025-10-24 20:10:00 +0900
categories: [Mobile, iOS]
tags: [uikit, segue, present, dismiss, viewcontroller, storyboard, prepare, performSegue]
---

## **화면 전환 및 데이터 전달(간접 세그웨이)**

스토리보드에서 전환할 `ViewController`를 세그웨이로 연결해줘야함.

> `Segue`(세그웨이)는 화면 전환을 처리하는 객체임.
{: .prompt-tip }

1.`ViewController`를 클린한 뒤 키보드에 `Control` 키를 누른 상태에서 전환할 `ViewController`로 드래그 후 `PresentModally`를 선택.

![image](/assets/img/storyboardseguesetting1.png)

2.`Segue` 확인

![image](/assets/img/storyboardseguecheck.png)

3.`Segue`의 `Identifier` 값을 설정

![image](/assets/img/storyboardseguesetting2.png)

위 값은 `Segue`를 통해 화면 전환을 처리할 때 필요함.

**화면 코드 구현**

`ViewController.swift`
```swift
import UIKit

class ViewController: UIViewController {

    override func viewDidLoad() {
        super.viewDidLoad()
    }

    @IBAction func nextBtnTapped(_ sender: UIButton) {
        // 세그웨이 활성화 메서드
        performSegue(withIdentifier: "nextSegue", sender: self)
    }
    
    // 데이터 전달 처리를 위해 prepare 메서드를 오버라이딩 해줌.
    // performSegue가 호출된 직후, 실제 화면 전환 전에 prepare 메서드가 자동 호출되어 데이터를 주입할 수 있음.
    override func prepare(for segue: UIStoryboardSegue, sender: Any?) {
        
        // 세그웨이 ID 값 확인
        if segue.identifier == "nextSegue"{
            
            // destination: 전환되는 화면
            let nextVC = segue.destination as! NextViewController
            
            // 데이터 전달
            nextVC.text = "Hello World!"
        }
    }
}
```

`NextViewController.swift`
```swift
import UIKit

class NextViewController: UIViewController {
    
    @IBOutlet weak var exLabel: UILabel!
    
    // 데이터를 전달받기 위한 변수
    var text: String?
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        exLabel.text = text
    }
    
    @IBAction func backBtnTapped(_ sender: UIButton) {
        // 이전 화면으로 돌아가기
        dismiss(animated: true)
    }
}
```

**결과**

![image](/assets/img/storyboardsegueresult.gif)

위 예시처럼 버튼같은 UI에 직접 세그웨이를 연결하는 것이 아니라 코드(`performSegue`)로 세그웨이를 실행하는 방식을 간접 세그웨이라고 함.