---
title: "UIKit - 화면 전환 및 데이터 전달(직접 세그웨이)"
date: 2025-10-25 10:08:00 +0900
categories: [Mobile, iOS]
tags: [uikit, segue, present, dismiss, viewcontroller, storyboard, prepare, shouldPerformSegue]
---

## **화면 전환 및 데이터 전달(직접 세그웨이)**
간접 세그웨이와는 다르게 직접 세그웨이는 버튼같은 UI 요소를 다른 `ViewController`에 직접 연결하는 것임.

**1.스토리보드 버튼 UI를 `ViewController`에 연결 (Present Modally 사용)**

![image](/assets/img/storyboardsegue2connecttry.png)

**2.연결된 세그웨이 확인**

![image](/assets/img/storyboardsegue2check.png)

**3.세그웨이 ID 설정**

![image](/assets/img/storyboardsegue2idsetting.png)

특이한 점은 간접 세그웨이는 `performSegue` 메서드를 통해 화면 전환을 처리해줬지만, 직접 세그웨이는 스토리보드 연결만으로 자동으로 전환됨.

**결과**

![image](/assets/img/segue2result1.gif)

이전 화면으로 돌아가기, 데이터 전달 등은 직접 구현이 필요함.

`ViewController.swift`
```swift
import UIKit

class ViewController: UIViewController {

    override func viewDidLoad() {
        super.viewDidLoad()
    }

    // 데이터 전달 처리
    override func prepare(for segue: UIStoryboardSegue, sender: Any?) {
        
        // 세그웨이 ID 검증
        guard segue.identifier == "nextSegue" else {
            print("다른 세그웨이임")
            return
        }
        
        let nextVC = segue.destination as! NextViewController
        nextVC.text = "응애" // 데이터 전달
    }
}
```

`NextViewController.swift`
```swift
import UIKit

class NextViewController: UIViewController {

    @IBOutlet weak var exLabel: UILabel!
    
    // 데이터를 전달받을 변수
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

![image](/assets/img/segue2result2.gif)

간접 세그웨이와 또 다른 점은 `shouldPerformSegue` 메서드를 오버라이딩해서 화면 전환 처리 여부를 결정할 수 있음.

`ViewController.swift`
```swift
import UIKit

class ViewController: UIViewController {

    override func viewDidLoad() {
        super.viewDidLoad()
    }

    // 데이터 전달 처리
    override func prepare(for segue: UIStoryboardSegue, sender: Any?) {
        
        // 세그웨이 ID 검증
        guard segue.identifier == "nextSegue" else {
            print("다른 세그웨이임")
            return
        }
        
        let nextVC = segue.destination as! NextViewController
        nextVC.text = "응애" // 데이터 전달
    }
    
    // 직접 세그웨이 화면 전환 여부 처리
    override func shouldPerformSegue(withIdentifier identifier: String, sender: Any?) -> Bool {
        if identifier == "nextSegue"{
            return false // nextSegue는 화면 전환을 처리하지 않음.
        }else{
            return true // 다른 ID일 경우 화면 전환
        }
    }
}
```

**결과**

![image](/assets/img/segue2result3.gif)

> 간접 세그웨이는 코드로 `performSegue`를 실행하고, 직접 세그웨이는 자동으로 `shouldPerformSegue`를 호출해 전환 여부를 판단함.
{: .prompt-tip }