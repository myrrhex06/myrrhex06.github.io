---
title: "UIKIt - UITableView Section 나누기"
date: 2026-03-12 06:50:00 +0900
categories: [Mobile, iOS]
tags: [ios, uikit, uitableview, section]
---

`UITableView`에서 `Section`을 분리하려면 `UITableViewDataSource`의 메서드를 구현하면 됨.

사용 메서드

| 메서드                                | 설명                              |
| ------------------------------------- | --------------------------------- |
| `numberOfSections(in:)`               | `Section` 개수 반환               |
| `tableView(_:numberOfRowsInSection:)` | 특정 `Section`의 행 개수 반환     |
| `tableView(_:cellForRowAt:)`          | 특정 `Section`의 행과 `Cell` 반환 |



`SettingViewController.swift`
```swift
final class SettingViewController: UIViewController {
    
    // 요소 정의
    private let cellList: [[SettingCellDto]] = [
        [
            SettingCellDto(text: "정보 수정", image: UIImage(systemName: "person.fill", withConfiguration: UIImage.SymbolConfiguration(pointSize: 22))),
            SettingCellDto(text: "비밀번호 변경", image: UIImage(systemName: "lock.fill", withConfiguration: UIImage.SymbolConfiguration(pointSize: 22)))
        ],
        [
            SettingCellDto(text: "로그아웃", image: UIImage(systemName: "rectangle.portrait.and.arrow.right", withConfiguration: UIImage.SymbolConfiguration(pointSize: 22)))
        ]
    ]

}

extension SettingViewController: UITableViewDelegate, UITableViewDataSource{
    
    // Section 개수 반환
    func numberOfSections(in tableView: UITableView) -> Int {
        return cellList.count
    }
    
    // 특정 Section의 행 개수 반환
    func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        return cellList[section].count
    }
    
    // 특정 Section의 행과 Cell 반환
    func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        let cell = tableView.dequeueReusableCell(withIdentifier: SettingTableViewCell.identifier, for: indexPath) as! SettingTableViewCell
        let cellData = cellList[indexPath.section][indexPath.row]
        cell.iconText.textLabel.text = cellData.text
        cell.iconText.iconImage.image = cellData.image
        
        return cell
    }
}
```

결과

![image](/assets/img/tableviewdividesectionresult.gif)

## **Reference**
- [https://yeoseongil.tistory.com/141](https://yeoseongil.tistory.com/141)