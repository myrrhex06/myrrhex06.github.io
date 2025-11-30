---
title: "UIKit - UITableView EmptyView 구현하기"
date: 2025-11-30 09:55:00 +0900
categories: [Mobile, iOS]
tags: [uikit, code, uitableview, extension]
---

`UITableView`에 표시할 데이터가 없으면 아래 화면처럼 텅빈 화면만 나타남.

![image](/assets/img/uitableviewemptydata.gif)

`TableView`에 표시할 데이터가 없을 때에만 렌더링되는 `View`를 따로 만들어 `TableView`의 `backgroundView`로 설정해주는 것으로 해결할 수 있음.

`UITableView+EmptyView.swift`
```swift
import UIKit

extension UITableView{
    
    // EmptyView 설정
    func setupEmptyView(message: String){

        let emptyView: UIView = UIView(frame: CGRect(x: self.center.x,
                                                     y: self.center.y,
                                                     width: self.bounds.size.width,
                                                     height: self.bounds.size.height))
        
        let messageLabel: UILabel = {
            let label = UILabel()
            
            label.translatesAutoresizingMaskIntoConstraints = false
            label.text = message
            label.textColor = .createdDate
            label.numberOfLines = 0
            label.textAlignment = .center
            label.font = UIFont.systemFont(ofSize: 17, weight: .medium)
            label.sizeToFit()
            
            return label
        }()
        
        self.addSubview(emptyView)
        
        emptyView.addSubview(messageLabel)
        
        NSLayoutConstraint.activate([
            messageLabel.centerXAnchor.constraint(equalTo: emptyView.centerXAnchor),
            messageLabel.centerYAnchor.constraint(equalTo: emptyView.centerYAnchor)
        ])
        
        // TableView의 backgroundView로 설정
        self.backgroundView = emptyView
    }
    
    // EmptyView 제거
    func removeEmptyView(){
        self.backgroundView = nil
    }
}
```

위 코드처럼 `UITableView`를 확장하여 `EmptyView`를 추가/삭제 하는 메서드를 구현해준 뒤 `TableView DataSource`에서 표시할 `Cell`의 개수가 `0`일 경우에는 `EmptyView`를 렌더링하도록 적절하게 설정해주면 됨.

`ViewController.swift`
```swift
// MARK: - 테이블뷰 DataSource
extension ViewController: UITableViewDataSource{
    
    func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        guard let todoList = coreDataManager.getTodoList() else {
            tableView.setupEmptyView(message: "Your task list is empty.")
            return 0
        }
        
        if todoList.count == 0 {
            tableView.setupEmptyView(message: "Your task list is empty.")
        }else{
            tableView.removeEmptyView()
        }
        
        return todoList.count
    }

}
```

결과

![image](/assets/img/applyuitableviewemptyview.gif)

## **Reference**

- [https://chanhee-jeong.tistory.com/m/5](https://chanhee-jeong.tistory.com/m/5)