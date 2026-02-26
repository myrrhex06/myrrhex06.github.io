---
title: "UIKit - navigation push를 통해 화면 전환 시 UITabBar 숨기기"
date: 2026-02-27 06:40:00 +0900
categories: [Mobile, iOS]
tags: [ios, uikit, uitabbarcontroller, uinavigationcontroller]
---

특정 화면에서 `tabbar`를 숨기기 위해선 아래와 같이 `hidesBottomBarWhenPushed` 프로퍼티를 통해 처리 가능함.

예시
```swift
extension BookListViewController: UITableViewDataSource, UITableViewDelegate{
    
    func tableView(_ tableView: UITableView, didSelectRowAt indexPath: IndexPath) {
        let detailVC = BookDetailViewController()
        detailVC.hidesBottomBarWhenPushed = true
        self.navigationController?.pushViewController(detailVC, animated: true)
    }

    ...

}
```

> `tabBarController`에서 제공하는 `isHidden` 프로퍼티는 TabBar 영역은 그대로 유지하지만 `hidesBottomBarWhenPushed`는 영역도 숨겨줌
{: .prompt-tip }

결과

![image](/assets/img/tabbarhideresult.gif)

