---
title: "UIKit - NotificationCenter를 활용하여 키보드 높이만큼 View 올리기, 내리기"
date: 2026-01-19 18:55:00 +0900
categories: [Mobile, iOS]
tags: [ios, uikit, uitextfield, delegate, return, button]
---

한 `View`에 여러 개의 `UITextField`를 배치하게 되면 키보드가 올라왔을 때 가려지는 현상이 발생하게 됨.

이런 문제를 `NotificationCenter`를 통해 키보드가 올라왔을 때, 내려왔을 때를 감지하여 `View`를 올려줌으로써 해결할 수 있음.

내 경우에는 닉네임을 입력하는 `UITextField`가 `firstResponder`가 됬을 때만 `View`가 올라가도록 하고, 나머지는 그대로 유지하도록 처리함.

키보드 여부, 기존 `origin` 값의 상태 관리를 위해 `Model`을 하나 정의해줌.

`RegisterViewManager.swift`
```swift
import Foundation

final class RegisterViewManager{
    
    public static let shared = RegisterViewManager()
    
    private init() {}
    
    // 키보드 여부
    private var isKeyboard: Bool = false
    
    // 기존 origin Y 값
    private var originY: CGFloat?
    
    func setIsKeyboard(_ value: Bool){
        self.isKeyboard = value
    }
    
    func getIsKeyboard() -> Bool{
        return self.isKeyboard
    }
    
    func setOriginY(_ value: CGFloat?){
        self.originY = value
    }
    
    func getOriginY() -> CGFloat?{
        return self.originY
    }
}
```

`RegisterViewController.swift`
```swift
import UIKit

final class RegisterViewController: UIViewController {
    
    private let registerView = RegisterView()
    
    private let registerViewManager = RegisterViewManager.shared
    
    override func viewDidLoad() {
        super.viewDidLoad()

        // NotificationCenter Observe 설정
        setKeyboardObserver()

        // origin Y축 값 설정
        registerViewManager.setOriginY(self.view.frame.origin.y)
    }

    override func viewWillDisappear(_ animated: Bool) {
        super.viewWillDisappear(animated)

        // 설정된 NotificationCenter Observe 해제
        removeKeyboardObserver()
    }
}

// MARK: - Notification Center
extension RegisterViewController{
    
    func setKeyboardObserver(){
      // 키보드가 올라오기 전에 처리할 핸들러 등록
        NotificationCenter.default.addObserver(self, selector: #selector(keyboardWillShow), name: UIResponder.keyboardWillShowNotification, object: nil)
        
      // 키보드가 내려가기 전에 처리할 핸들러 등록
        NotificationCenter.default.addObserver(self, selector: #selector(keyboardWillHide), name: UIResponder.keyboardWillHideNotification, object: nil)
    }
    
    func removeKeyboardObserver(){
      // deinit 시점에 설정한 핸들러 제거

        NotificationCenter.default.removeObserver(self, name: UIResponder.keyboardWillShowNotification, object: nil)
      
        NotificationCenter.default.removeObserver(self, name: UIResponder.keyboardWillHideNotification, object: nil)
    }
    
    @objc func keyboardWillShow(notification: NSNotification){
        
        // 닉네임 TextField가 firstResponder일 경우
        if registerView.getNicknameTextField().isFirstResponder{
            
            // 키보드가 올라와있다면 return
            guard !registerViewManager.getIsKeyboard() else { return }
            
            // 키보드 높이 구하기
            if let keyboardFrame = notification.userInfo?[UIResponder.keyboardFrameEndUserInfoKey] as? NSValue{
                let keyboardRectangle = keyboardFrame.cgRectValue
                let keyboardHeight = keyboardRectangle.height
                
                // View 올리기
                UIView.animate(withDuration: 0.5) {
                    self.view.frame.origin.y -= keyboardHeight
                }
                
                // isKeyboard 프로퍼티 true 설정
                registerViewManager.setIsKeyboard(true)
            }
        }else{
            // 만약 닉네임 TextField가 아닌 다른 TextField 요소가 firstResponder가 됬을 때 실행

            // 기존 origin 값과 현재 origin 값이 같지 않다면
            if self.view.frame.origin.y != registerViewManager.getOriginY(){
                
                // 키보드 높이를 구한 후 View를 내려줌
                if let keyboardFrame = notification.userInfo?[UIResponder.keyboardFrameEndUserInfoKey] as? NSValue{
                    let keyboardRectangle = keyboardFrame.cgRectValue
                    let keyboardHeight = keyboardRectangle.height
                    
                    UIView.animate(withDuration: 0.5) {
                        self.view.frame.origin.y += keyboardHeight
                    }
                    
                    registerViewManager.setIsKeyboard(false)
                    registerViewManager.setOriginY(self.view.frame.origin.y)
                }
                
            }
        }
    }
    
    @objc func keyboardWillHide(notification: NSNotification){
      // 닉네임 TextField가 firstResponder라면 실행
        if registerView.getNicknameTextField().isFirstResponder{
            
            guard registerViewManager.getIsKeyboard() else { return }
            
            // 키보드 높이를 구한 뒤 View를 내려줌
            if let keyboardFrame = notification.userInfo?[UIResponder.keyboardFrameEndUserInfoKey] as? NSValue{
                let keyboardRectangle = keyboardFrame.cgRectValue
                let keyboardHeight = keyboardRectangle.height
                
                UIView.animate(withDuration: 0.5) {
                    self.view.frame.origin.y += keyboardHeight
                }
                
                registerViewManager.setIsKeyboard(false)
            }
        }
    }   
}
```

결과

![image](/assets/img/updownkeyboardresult.gif)