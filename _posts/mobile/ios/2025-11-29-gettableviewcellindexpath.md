---
title: "UIKit - UITableViewCell IndexPath 얻기"
date: 2025-11-29 20:32:00 +0900
categories: [Mobile, iOS]
tags: [uikit, code, uitableview, uitableviewcell, indexpath]
---

특정 `UITableViewCell` 객체의 `IndexPath`를 얻기 위해선 `UITableView` 객체의 `indexPath(for:)` 메서드를 통해 처리 가능함.

![image](/assets/img/indexpathformethod.png)

`ViewController.swift`
```swift
// MARK: - TodoCell Delegate
extension ViewController: TodoCellDelegate{
    
    func completedButtonTapped(cell: TodoCell) {
        guard let todo = cell.getTodoData() else { return }
        
        ...

        guard let indexPath = tableView.indexPath(for: cell) else { return }

        ...
    }
}
```
