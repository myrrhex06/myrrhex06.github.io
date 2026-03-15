---
title: "UIKIt - UITableView 동적 height 설정하기"
date: 2026-03-16 06:38:00 +0900
categories: [Mobile, iOS]
tags: [ios, uikit, uitableview, cell, height]
---

예시
```swift
import UIKit

final class SettingViewController: UIViewController {
    
    private lazy var tableView: UITableView = {
        let tableView = UITableView(frame: .zero, style: .insetGrouped)
        
        // 셀 높이를 내부 컨텐츠에 맞춰 자동 조정
        tableView.rowHeight = UITableView.automaticDimension
        
        // 미리 추정 높이 설정
        tableView.estimatedRowHeight = 100
        
        return tableView
    }()
}
```

`Cell` 내부 UI 요소에서 마지막 요소에는 반드시 `bottom` 제약을 줘야함.

## **Reference**
- [https://justhm.tistory.com/87](https://justhm.tistory.com/87)