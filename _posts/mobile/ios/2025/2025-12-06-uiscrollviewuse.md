---
title: "UIKit - UIScrollView 사용하기"
date: 2025-12-06 16:30:00 +0900
categories: [Mobile, iOS]
tags: [uikit, code, uiscrollview, autolayout]
---

> 이 게시글은 `UIScrollView`의 개념이 코드 베이스로 `UIScrollView`를 사용하는 것을 기록함.

`UIScrollView`는 화면을 스크롤 하거나, 확대/축소할 수 있게 해줌.

> `UITableView`, `UICollectionView` 모두 `UIScrollView`를 상속하고 있음.
{: .prompt-tip }

`TodoDetailView.swift`
```swift
import UIKit

class TodoDetailView: UIView {
    
    // MARK: - UIScrollView 인스턴스 생성
    private let scrollView: UIScrollView = {
        let scrollView = UIScrollView()
        
        scrollView.translatesAutoresizingMaskIntoConstraints = false
        
        return scrollView
    }()

    // MARK: - UIScrollView에 적용시킬 ContentView 인스턴스 생성
    private let contentView: UIView = {
        let view = UIView()
        
        view.translatesAutoresizingMaskIntoConstraints = false
        
        return view
    }()

    ...

    override init(frame: CGRect) {
        super.init(frame: frame)
        
        setupUI()
    }

    private func setupUI(){
        ...
        
        self.addSubview(scrollView) // UIScrollView 추가
        scrollView.addSubview(contentView) // UIScrollView에 ContentView 추가
        
        // ContnetView 하위에 표시할 요소 추가
        contentView.addSubview(todoTitleLabel)
        contentView.addSubview(createdDateLabel)
        contentView.addSubview(completedStackView)
        contentView.addSubview(descriptionStackView)

        // 오토레이아웃 설정
        setupScrollView()

        ...
    }

    private func setupScrollView(){
        NSLayoutConstraint.activate([
          // UIScrollView 화면에 꽉 차게 설정
            scrollView.leadingAnchor.constraint(equalTo: self.leadingAnchor),
            scrollView.trailingAnchor.constraint(equalTo: self.trailingAnchor),
            scrollView.topAnchor.constraint(equalTo: self.topAnchor),
            scrollView.bottomAnchor.constraint(equalTo: self.bottomAnchor),
            
          // ContentView UIScrollView 크기에 맞게 설정
            contentView.leadingAnchor.constraint(equalTo: self.scrollView.leadingAnchor),
            contentView.trailingAnchor.constraint(equalTo: self.scrollView.trailingAnchor),
            contentView.topAnchor.constraint(equalTo: self.scrollView.topAnchor),
            contentView.bottomAnchor.constraint(equalTo: self.scrollView.bottomAnchor),
            contentView.widthAnchor.constraint(equalTo: self.scrollView.widthAnchor)
        ])
    }

    ...

    private func setupDescriptionStackView(){
        NSLayoutConstraint.activate([
            displayDescriptionLabel.heightAnchor.constraint(equalToConstant: 30),
            
            descriptionStackView.leadingAnchor.constraint(equalTo: contentView.leadingAnchor, constant: 10),
            descriptionStackView.trailingAnchor.constraint(equalTo: contentView.trailingAnchor, constant: -10),
            descriptionStackView.topAnchor.constraint(equalTo: completedStackView.bottomAnchor, constant: 30),

            // UIStackView와 같이 동적으로 크기가 조정되는 요소는 UIScrollView에 표시되는 마지막 요소에는 bottomAnchor 설정이 필수적으로 필요함
            descriptionStackView.bottomAnchor.constraint(equalTo: contentView.bottomAnchor, constant: -30)
        ])
    }
}
```

결과

![image](/assets/img/uiscrollviewapply.gif)

## **Reference**
- [https://apple-pecanpie.tistory.com/7](https://apple-pecanpie.tistory.com/7)
- [https://dev-dain.tistory.com/215](https://dev-dain.tistory.com/215)
- [https://iosdevlime.tistory.com/entry/iOSUIKit-UIScrollView-AutoLayout-%EA%B5%AC%ED%98%84%ED%95%98%EA%B8%B0-With-Scrollable-Content-Size-Ambiguity-%EC%97%90%EB%9F%AC-%ED%95%B4%EA%B2%B0](https://iosdevlime.tistory.com/entry/iOSUIKit-UIScrollView-AutoLayout-%EA%B5%AC%ED%98%84%ED%95%98%EA%B8%B0-With-Scrollable-Content-Size-Ambiguity-%EC%97%90%EB%9F%AC-%ED%95%B4%EA%B2%B0)
- [https://beepeach.tistory.com/613](https://beepeach.tistory.com/613)