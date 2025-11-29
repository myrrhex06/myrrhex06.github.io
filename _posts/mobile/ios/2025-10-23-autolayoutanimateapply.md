---
title: "UIKit - 오토 레이아웃 동적 변경 애니메이션 구현 방법"
date: 2025-10-23 20:24:00 +0900
categories: [Mobile, iOS]
tags: [uikit, code, autolayout, animate, constraint, uiview]
---

> 2025년 11월 29일 내용 업데이트함.

## **오토 레이아웃 동적 변경 애니메이션 구현 방법**

사이드 프로젝트를 진행하던 도중 아래와 같이 키보드가 올라오면 구현해둔 `TextView`를 가리는 문제가 발생함.

![image](/assets/img/beforedynamicautolayoutandnotificationcenter.gif)

이 문제를 해결하기 위해 키보드가 올라오면 구현해둔 `TextView`, `TextField`를 올라가도록 처리하기로 결정하였음.

`NotificationCenter`를 통해 키보드가 올라오는 이벤트를 수신하였고, 이제 오토레이아웃 제약 조건을 변경해준 후에 `layoutIfNeeded`를 통해 화면을 다시 그려주면 되었으나.......

기존에 정리해둔 이 글에 내용이 너무 빈약해서 구현한 내용을 다시 정리하기로 함.

`TodoAddView.swift`
```swift
import UIKit

class TodoAddView: UIView {

    ...

    // 오토레이아웃 제약을 저장하는 프로퍼티
    private lazy var todoTitleTopConstraint: NSLayoutConstraint = todoTitleTextField.topAnchor.constraint(equalTo: self.topAnchor, constant: 150)


    ...


    // 오토레이아웃 설정 처리 메서드
    func setupTodoTitleTextField(){
        self.addSubview(todoTitleTextField)
        
        NSLayoutConstraint.activate([
            todoTitleTextField.leadingAnchor.constraint(equalTo: self.leadingAnchor, constant: 20),
            todoTitleTextField.trailingAnchor.constraint(equalTo: self.trailingAnchor, constant: -20),
            todoTitleTopConstraint,
            todoTitleTextField.heightAnchor.constraint(equalToConstant: 80)
        ])
    }

    ...


    // 키보드가 올라왔을 때 실행
    func keyboardWillShow(){

        // 제약 조건 설정
        todoTitleTopConstraint.constant -= 30
        
        // 화면이 다시 그려지는 과정에 애니메이션을 적용
        UIView.animate(withDuration: 0.3) {
            
            self.layoutIfNeeded()
        }
    }
    
    // 키보드가 내려갔을 때 실행
    func keyboardWillHide(){

        // 제약 조건 설정
        todoTitleTopConstraint.constant += 30
        
        // 화면이 다시 그려지는 과정에 애니메이션을 적용
        UIView.animate(withDuration: 0.3) {
            
            self.layoutIfNeeded()
        }
    }
}
```

> `layoutIfNeeded`: 변경된 제약조건을 즉시 반영하도록 강제로 Layout을 업데이트함.
{: .prompt-tip }

`TodoAddViewController.swift`
```swift
import UIKit

final class TodoAddViewController: UIViewController {

    // 할일 추가 화면
    let todoAddView = TodoAddView()

    ....

    // 키보드가 올라왔을 때 실행
    @objc func keyboardWillShow(){
        print(#function)
        
        // 오토레이아웃 제약 변경 메서드 실행
        todoAddView.keyboardWillShow()
    }
    
    // 키보드가 내려왔을 때
    @objc func keyboardWillHide(){
        print(#function)
        
        // 오토레이아웃 제약 변경 메서드 실행
        todoAddView.keyboardWillHide()
    }
}
```

결과

![image](/assets/img/afterdynamicautolayoutandnotificationcenter.gif)