---
title: "UIKit - 화면 전환 및 데이터 전달(코드 및 스토리보드 모두 사용해서 구현)"
date: 2025-10-24 18:15:00 +0900
categories: [Mobile, iOS]
tags: [uikit, code, present, dismiss, viewcontroller, storyboard]
---

## **화면 전환 및 데이터 전달(코드 및 스토리보드 모두 사용해서 구현)**
스토리보드에 `ViewController`를 생성한 후 `ViewController` 파일과 연결해야함.

스토리보드 `ViewController` 구성 예시

![image](/assets/img/storyboardviewconrollerexample.png)

`ViewController` 연결 예시

![image](/assets/img/storyboardviewcontrollerconnect.png)

> `Storyboard ID`는 스토리보드에 있는 `ViewController`를 코드로 가져올 때 필요하기 때문에 지정해줘야함.
{: .prompt-tip }

**화면 전환 코드 구성**

`ViewController.swift`
```swift
import UIKit

final class ViewController: UIViewController {

    override func viewDidLoad() {
        super.viewDidLoad()
    }

    @IBAction func convertButtonTapped(_ sender: UIButton) {
        
        // 스토리보드의 ViewController를 가져옴.
        let nextVC = storyboard?.instantiateViewController(withIdentifier: "NextVC") as! NextViewController
        
        nextVC.text = "Hello World!"
        nextVC.modalPresentationStyle = .fullScreen
        
        present(nextVC, animated: true)
    }
}
```

코드로만 구현했을 떄와는 달리 스토리보드를 사용할 때는 `NextViewController`의 인스턴스를 직접 생성할 수 없으며 `UIViewController`에 정의되어 있는 `storyboard` 프로퍼티를 사용해서 스토리보드에 접근해서 가져와야함.

이때 `withIdentifier` 값은 위에서 설정한 `Storyboard ID` 값을 넣어주면 됨.

반환값이 `UIViewController` 이기 때문에 형변환을 해줌.

`NextViewController.swift`
```swift
import UIKit

class NextViewController: UIViewController {

    @IBOutlet weak var exLabel: UILabel!
    
    // 데이터 전달용 변수
    var text: String?
    
    override func viewDidLoad() {
        super.viewDidLoad()

        exLabel.text = text
    }

    @IBAction func backBtnTapped(_ sender: UIButton) {
        
        // 이전 화면으로 되돌아가기
        dismiss(animated: true)
    }
}
```

**결과**

![image](/assets/img/displayconvertbycodeandstoryboardresult.gif)