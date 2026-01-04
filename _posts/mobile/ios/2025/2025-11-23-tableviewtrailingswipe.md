---
title: "UIKit - UITableView 스와이프 버튼 구현하기"
date: 2025-11-23 13:46:00 +0900
categories: [Mobile, iOS]
tags: [uikit, code, uitableview, cell, swipe, delegate]
---

`UITableView`의 `Delegate` 메서드를 구현하여 스와이프 액션을 처리할 수 있음.

`ViewController.swift`
```swift
import UIKit

final class ViewController: UIViewController {

    // UITableView 인스턴스 생성
    private let tableView = UITableView()
    
    ...


    override func viewDidLoad() {
      super.viewDidLoad()

      // viewDidLoad 시점에 호출
      setupTableView()
    }


    func setupTableView(){
        ....
        
        // Delegate 설정
        tableView.delegate = self
        
        ...
    }
}

// MARK: - 테이블뷰 Delegate
extension ViewController: UITableViewDelegate{
    
    // 오른쪽 스와이프 처리
    func tableView(_ tableView: UITableView, trailingSwipeActionsConfigurationForRowAt indexPath: IndexPath) -> UISwipeActionsConfiguration? {
        
        // 삭제 버튼 설정
        let deleteButton = UIContextualAction(style: .destructive, title: nil) { _, _, success in
            print("\(indexPath.row) 삭제 버튼 클릭")
            success(true)
        }
        
        deleteButton.image = UIImage(systemName: "trash")
        
        return UISwipeActionsConfiguration(actions: [deleteButton])
    }
}
```

결과

![image](/assets/img/tableviewtrailingswiperesult.gif)

## **Reference**
- [https://how-mrk.tistory.com/entry/Swift-%ED%85%8C%EC%9D%B4%EB%B8%94%EB%B7%B0-%EC%8A%A4%EC%99%80%EC%9D%B4%ED%94%84-%EB%B2%84%ED%8A%BCTableView-swipe-button](https://how-mrk.tistory.com/entry/Swift-%ED%85%8C%EC%9D%B4%EB%B8%94%EB%B7%B0-%EC%8A%A4%EC%99%80%EC%9D%B4%ED%94%84-%EB%B2%84%ED%8A%BCTableView-swipe-button)