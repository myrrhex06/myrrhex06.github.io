---
title: "UIKit - UITableView Section Header 설정하기"
date: 2026-02-03 06:42:00 +0900
categories: [Mobile, iOS]
tags: [ios, uikit, tableview, section, header]
---

예시 코드

```swift
import UIKit

final class BookListViewController: UIViewController {
    
    // Section Header 배열 정의
    private let tableViewSectionHeader = ["책 기록"] 
}

extension BookListViewController: UITableViewDataSource, UITableViewDelegate{

    // Section 수
    func numberOfSections(in tableView: UITableView) -> Int {
        return tableViewSectionHeader.count
    }
    
    // Section Title Text
    func tableView(_ tableView: UITableView, titleForHeaderInSection section: Int) -> String? {
        return tableViewSectionHeader[section]
    }
    
    // Section Title 폰트 설정
    func tableView(_ tableView: UITableView, viewForHeaderInSection section: Int) -> UIView? {
        let titleLabel = UILabel()
        titleLabel.font = UIFont.boldSystemFont(ofSize: 24)
        titleLabel.textColor = .white
        titleLabel.text = self.tableView(tableView, titleForHeaderInSection: section)
        titleLabel.textAlignment = .left
        
        let headerView = UIView()
        headerView.addSubview(titleLabel)
        headerView.backgroundColor = .background
        
        titleLabel.snp.makeConstraints { make in
            make.centerY.equalToSuperview()
            make.leading.equalToSuperview().inset(5)
        }
        
        return headerView
    }

    // Header 높이 설정
    func tableView(_ tableView: UITableView, heightForHeaderInSection section: Int) -> CGFloat {
        return 50
    }
}
```

결과

![image](/assets/img/uitableviewsectionheadertitle.gif)

## **Reference**
- [https://nlestory.tistory.com/253](https://nlestory.tistory.com/253)