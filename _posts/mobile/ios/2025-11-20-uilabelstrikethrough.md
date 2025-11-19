---
title: "UIKit - UILabel 취소선 적용하기"
date: 2025-11-20 06:43:00 +0900
categories: [Mobile, iOS]
tags: [uikit, code, uilabel, nsattributedstring, nsmutableattributedstring]
---

`UILabel`에 취소선을 적용하는 것은 `NSAttributedString`을 통해 처리 가능함.

**Apple Develop Document**

![image](/assets/img/nsattributedstringdocument.png)

적용 코드

`String+StrikeThrough.swift`
```swift
import UIKit

extension String{
    
    // 취소선 적용
    func strikeThrough() -> NSAttributedString{
        let attributeString = NSMutableAttributedString(string: self)
        attributeString.addAttribute(.strikethroughStyle, value: NSUnderlineStyle.single.rawValue, range: NSMakeRange(0, attributeString.length))
        return attributeString
    }
    
    // 적용된 속성 해제
    func unStrikeThrough() -> NSAttributedString{
        let attributeString = NSMutableAttributedString(string: self)
        attributeString.addAttribute(.strikethroughStyle, value: [], range: NSMakeRange(0, attributeString.length))
        return attributeString
    }
}
```

`TodoCell.swift`
```swift
import UIKit

class TodoCell: UITableViewCell {
  ...

    func toggleLabelStrikeThrough(isCompleted: Bool){
        if isCompleted{
          // 완료 시 취소선 적용
            todoTitleLabel.textColor = UIColor(named: "completedColor")
            todoTitleLabel.attributedText = todoTitleLabel.text?.strikeThrough()
            
            createdAtLabel.textColor = UIColor(named: "completedColor")
            createdAtLabel.attributedText = createdAtLabel.text?.strikeThrough()
        }else{
          // 취소선 속성 해제
            todoTitleLabel.textColor = UIColor(named: "todoTitleColor")
            todoTitleLabel.attributedText = todoTitleLabel.text?.unStrikeThrough()
            
            createdAtLabel.textColor = UIColor(named: "createdDateColor")
            createdAtLabel.attributedText = createdAtLabel.text?.unStrikeThrough()
        }
    }

}
```

결과

![image](/assets/img/strikethroughapplyresult.gif)

## **Reference**
- [https://stackoverflow.com/questions/58628067/cant-reset-uilabel-attributedtext-when-a-uitableviewcell-is-reused](https://stackoverflow.com/questions/58628067/cant-reset-uilabel-attributedtext-when-a-uitableviewcell-is-reused)
- [https://velog.io/@gnwjd309/iOS-Strikethrough](https://velog.io/@gnwjd309/iOS-Strikethrough)