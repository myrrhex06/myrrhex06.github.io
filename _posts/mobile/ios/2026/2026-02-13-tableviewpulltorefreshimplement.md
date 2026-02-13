---
title: "UIKit - UITableView 당겨서 새로고침(Pull To Refresh) 구현하기"
date: 2026-02-13 20:23:00 +0900
categories: [Mobile, iOS]
tags: [ios, uikit, uirefreshcontrol, uitableview]
---

`UITableView`를 당겼을 때 새로고침 되는 기능을 구현하기 위해서는 `UIRefreshControl`을 사용하면 됨.

예시 코드
```swift
import UIKit

final class BookListViewController: UIViewController {
    
    private lazy var tableView: UITableView = {
        let tableView = UITableView()
        
        ...
        
        // RefreshControl 할당
        tableView.refreshControl = refreshControl
        
        return tableView
    }()
    
    private lazy var refreshControl: UIRefreshControl = {
        let refreshControl = UIRefreshControl()
        
        // 로딩바 색상 설정
        refreshControl.tintColor = .lightGray

        // refresh 시 호출할 메서드 설정
        refreshControl.addTarget(self, action: #selector(handleRefreshControl), for: .valueChanged)
        
        return refreshControl
    }()


    @objc private func handleRefreshControl(){
        bookListManager.list(page: 0, size: 10) { result in
            DispatchQueue.main.async {
                switch result{
                case .success(let response):
                    self.bookListManager.setBookList(response)
                    self.tableView.reloadData()
                case .failure(let error):
                    print("error : \(error)")
                }
                
                // 표시되는 로딩바 제거
                self.refreshControl.endRefreshing()
            }

        }
    }
}
```

결과 

![image](/assets/img/tableviewrefreshcontrolimplement.gif)

## **Reference**
- [https://jimmy-ios.tistory.com/6](https://jimmy-ios.tistory.com/6)
- [https://hururuek-chapchap.tistory.com/15](https://hururuek-chapchap.tistory.com/15)
- [https://hururuek-chapchap.tistory.com/22](https://hururuek-chapchap.tistory.com/22)