---
title: "UIKit - UITableViewCell 클릭 시 색상 변경 제거하기"
date: 2026-02-06 06:41:00 +0900
categories: [Mobile, iOS]
tags: [ios, uikit, tableview, cell]
---

현재는 아래와 같이 `TableViewCell`을 클릭할 경우 색상이 변경됨.

![image](/assets/img/beforetableviewcellselectionnone.gif)

이를 `selectionStyle` 프로퍼티를 설정하여 해결할 수 있음.

```swift
extension BookListViewController: UITableViewDataSource, UITableViewDelegate{
    
    func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        let cell = tableView.dequeueReusableCell(withIdentifier: BookListTableViewCell.identifier, for: indexPath) as! BookListTableViewCell
        
        ...
        
        // 클릭 시 나타나는 효과 제거
        cell.selectionStyle = .none
        
        return cell
    }
    
    
}
```

결과

![image](/assets/img/aftertableviewcellselectionnone.gif)

## **Reference**
- [https://gonslab.tistory.com/41#google_vignette](https://gonslab.tistory.com/41#google_vignette)