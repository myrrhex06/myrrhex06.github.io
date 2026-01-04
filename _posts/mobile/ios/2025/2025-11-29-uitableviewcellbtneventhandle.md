---
title: "UIKit - UITableViewCell 내부 버튼 이벤트 처리하기"
date: 2025-11-29 18:49:00 +0900
categories: [Mobile, iOS]
tags: [uikit, code, uitableview, delegate, protocol, uitableviewcell]
---

`UItableViewCell` 내부에 있는 버튼 이벤트를 처리하는 것은 커스텀 델리게이트 프로토콜을 정의하여 처리할 수 있음.

`TodoCellDelegate.swift`
```swift
import UIKit

protocol TodoCellDelegate{
    func completedButtonTapped(cell: TodoCell)
}
```

`TodoCell.swift`
```swift
import UIKit

class TodoCell: UITableViewCell {

    ...

    // 델리게이트 프로퍼티
    var delegate: TodoCellDelegate?

    // MARK: - 완료 여부 체크박스
    private lazy var isCompletedCheckbox: CheckboxButton = {

        ...
        
        // 이벤트 핸들러 설정
        btn.addTarget(self, action: #selector(isCompletedCheckboxTapped), for: .touchUpInside)
        
        return btn
    }()

    ...

    // Cell 내부 버튼이 눌렸을 때 동작할 메서드
    @objc func isCompletedCheckboxTapped(){
        print("버튼 클릭")

        // 커스텀 델리게이트를 통해 눌린 Cell 정보를 보내줌
        delegate?.completedButtonTapped(cell: self)
    }
}
```

`ViewController.swift`
```swift
// MARK: - 테이블뷰 DataSource
extension ViewController: UITableViewDataSource{
    
    func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        ...
    }
    
    func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        let cell = tableView.dequeueReusableCell(withIdentifier: "TodoCell", for: indexPath) as! TodoCell
        
        ...

        // Cell 커스텀 델리게이트 설정
        cell.delegate = self
        
        return cell
    }
}

// MARK: - TodoCell Delegate
extension ViewController: TodoCellDelegate{
    
    // Cell 커스텀 델리게이트 프로토콜 구현
    func completedButtonTapped(cell: TodoCell) {
        print("cell completed")
    }
}
```

## **Reference**
- [https://velog.io/@ekdlrkzm/UITableViewCell-%EB%82%B4%EB%B6%80-%EB%B2%84%ED%8A%BC-%ED%81%B4%EB%A6%AD-%EC%9D%B4%EB%B2%A4%ED%8A%B8](https://velog.io/@ekdlrkzm/UITableViewCell-%EB%82%B4%EB%B6%80-%EB%B2%84%ED%8A%BC-%ED%81%B4%EB%A6%AD-%EC%9D%B4%EB%B2%A4%ED%8A%B8)