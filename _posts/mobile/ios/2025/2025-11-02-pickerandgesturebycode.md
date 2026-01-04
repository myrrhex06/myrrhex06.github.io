---
title: "UIKit - 코드로 Picker, Gesture 설정하기"
date: 2025-11-02 17:06:00 +0900
categories: [Mobile, iOS]
tags: [uikit, code, picker, imageview, gesture]
---

## **코드로 Picker, Gesture 설정하기**
**예시**

`ViewController.swift`
```swift
import UIKit
import PhotosUI // PickerView를 사용하기 위해 import

class ViewController: UIViewController {

    private let imageView: UIImageView = {
        let imageView = UIImageView()
       
        imageView.backgroundColor = .systemGray5
        imageView.tintColor = .black
        
        imageView.translatesAutoresizingMaskIntoConstraints = false
        
        return imageView
    }()
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        makeUI()
        setGesture()
    }
    
    override func viewDidLayoutSubviews() {
        super.viewDidLayoutSubviews()
        
        imageView.layer.cornerRadius = imageView.frame.width / 2.0
        imageView.clipsToBounds = true
    }
    
    func makeUI(){
        view.addSubview(imageView)
        
        NSLayoutConstraint.activate([
            imageView.centerYAnchor.constraint(equalTo: view.centerYAnchor),
            imageView.centerXAnchor.constraint(equalTo: view.centerXAnchor),
            imageView.heightAnchor.constraint(equalToConstant: 200),
            imageView.widthAnchor.constraint(equalToConstant: 200)
        ])
    }
    
    func setGesture(){
        // 제스처 설정
        let gesture = UITapGestureRecognizer(target: self, action: #selector(imageViewTapped))
        imageView.addGestureRecognizer(gesture)
        
        // 사용자 터치 이벤트 활성화
        imageView.isUserInteractionEnabled = true
        
    }
    
    @objc func imageViewTapped(){
        print(#function)
        
        // Picker 설정
        var configuration = PHPickerConfiguration()
        
        // 사진 선택 가능 수
        configuration.selectionLimit = 0
        
        // 이미지만 선택할 수 있도록 필터링
        configuration.filter = .images
        
        // Picker 생성
        let picker = PHPickerViewController(configuration: configuration)
        
        // Picker Delegate 설정
        picker.delegate = self
        
        // Picker 띄우기
        present(picker, animated: true)
    }
}

// Picker 동작을 처리하기 위한 Delegate 프로토콜 구현
extension ViewController: PHPickerViewControllerDelegate{
    
    // 사진을 선택했을 때 실행
    func picker(_ picker: PHPickerViewController, didFinishPicking results: [PHPickerResult]) {
        // Picker 닫기
        picker.dismiss(animated: true)
        
        // results: 사용자가 선택한 사진 배열
        // first: results 배열 중 첫번째 요소를 가져옴
        // itemProvider: 선택한 사진 데이터를 실제 이미지 객체로 변환하는 역할
        let itemProvider = results.first?.itemProvider
        
        // 선택한 데이터가 UIImage로 변환 가능한지 확인
        if let itemProvider = itemProvider, itemProvider.canLoadObject(ofClass: UIImage.self){
            // loadObject: 실제 객체(UIImage 등)로 변환해줌
            itemProvider.loadObject(ofClass: UIImage.self){ (image, error) in
                
                // loadObject는 백그라운드에서 실행되는데, UI 업데이트는 메인 쓰레드에서 해야하기 때문에 DispatchQueue 사용
                DispatchQueue.main.async{
                    self.imageView.image = image as? UIImage
                }
            }
        }else{
            print("이미지 로드가 정상적으로 처리되지 않음.")
        }
    }
}
```

위 예시 코드를 분석해보겠음.

```swift
import PhotosUI // PickerView를 사용하기 위해 import
```

`Picker`를 사용하기 위해서는 `PhotosUI`를 `import` 해야함.

```swift
...

    func setGesture(){
        // 제스처 설정
        let gesture = UITapGestureRecognizer(target: self, action: #selector(imageViewTapped))
        imageView.addGestureRecognizer(gesture)
        
        // 사용자 터치 이벤트 활성화
        imageView.isUserInteractionEnabled = true
        
    }

...
```

`imageView`에 제스처를 설정하는 코드로, `imageView`는 기본적으로 터치같은 제스처 동작에 대한 처리가 되어있지 않기 때문에 제스처를 설정해줘야함.

```swift
...

    @objc func imageViewTapped(){
        print(#function)
        
        // Picker 설정
        var configuration = PHPickerConfiguration()
        
        // 사진 선택 가능 수
        configuration.selectionLimit = 0
        
        // 이미지만 선택할 수 있도록 필터링
        configuration.filter = .images
        
        // Picker 생성
        let picker = PHPickerViewController(configuration: configuration)
        
        // Picker Delegate 설정
        picker.delegate = self
        
        // Picker 띄우기
        present(picker, animated: true)
    }

...
```

`imageView`에 터치 제스처가 일어나면 실행되는 함수로, `Picker`를 설정하고 띄워줌.

```swift
// Picker 동작을 처리하기 위한 Delegate 프로토콜 구현
extension ViewController: PHPickerViewControllerDelegate{
    
    // 사진을 선택했을 때 실행
    func picker(_ picker: PHPickerViewController, didFinishPicking results: [PHPickerResult]) {
        // Picker 닫기
        picker.dismiss(animated: true)
        
        // results: 사용자가 선택한 사진 배열
        // first: results 배열 중 첫번째 요소를 가져옴
        // itemProvider: 선택한 사진 데이터를 실제 이미지 객체로 변환하는 역할
        let itemProvider = results.first?.itemProvider
        
        // 선택한 데이터가 UIImage로 변환 가능한지 확인
        if let itemProvider = itemProvider, itemProvider.canLoadObject(ofClass: UIImage.self){
            // loadObject: 실제 객체(UIImage 등)로 변환해줌
            itemProvider.loadObject(ofClass: UIImage.self){ (image, error) in
                
                // loadObject는 백그라운드에서 실행되는데, UI 업데이트는 메인 쓰레드에서 해야하기 때문에 DispatchQueue 사용
                DispatchQueue.main.async{
                    self.imageView.image = image as? UIImage
                }
            }
        }else{
            print("이미지 로드가 정상적으로 처리되지 않음.")
        }
    }
}
```

`Picker`에서 이미지를 선택한 후 동작을 처리하기 위해 Delegate 프로토콜을 구현함.

**결과**

![image](/assets/img/pickerandgesturebycoderesult.gif)