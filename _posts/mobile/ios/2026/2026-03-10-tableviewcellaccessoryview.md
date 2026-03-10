---
title: "UIKIt - UITableViewCell AccessoryView 설정하기"
date: 2026-03-10 20:27:00 +0900
categories: [Mobile, iOS]
tags: [ios, uikit, uitableviewcell, accessoryview]
---

`UITableView`에 표시되는 `Cell` 우측에 `>` 모양을 넣어야하는 상황이 생김. 

아래와 같이 단순히 `accessoryType` 프로퍼티 값을 `checkmark`로 설정해주면 화살표가 나타나긴 하지만 색상 조정이나 커스텀을 할 수가 없게 됨.

`SettingTableViewCell.swift`
```swift
import UIKit

final class SettingTableViewCell: UITableViewCell {

    public static let identifier = "SettingCell"
    
    ...

    override init(style: UITableViewCell.CellStyle, reuseIdentifier: String?) {
        super.init(style: style, reuseIdentifier: reuseIdentifier)
        
        self.accessoryType = .checkmark
    }

    ...
}
```

표시되는 화살표의 색상같이 직접 요소를 커스텀해줘야할 때는 `accessoryView`에 직접 `View`를 넣어서 처리할 수 있음.

`SettingTableViewCell.swift`
```swift
import UIKit

final class SettingTableViewCell: UITableViewCell {

    public static let identifier = "SettingCell"
    
    ...
    
    override init(style: UITableViewCell.CellStyle, reuseIdentifier: String?) {
        super.init(style: style, reuseIdentifier: reuseIdentifier)
        
        setupAccessoryView()
    }

    ...
    
    private func setupAccessoryView(){
        guard let image = UIImage(systemName: "greaterthan") else { return }
        let imageView = UIImageView(image: image)
        imageView.tintColor = .white
        accessoryView = imageView
    }

}
```

결과

![image](/assets/img/uitableviewcellaccessoryviewresult.gif)