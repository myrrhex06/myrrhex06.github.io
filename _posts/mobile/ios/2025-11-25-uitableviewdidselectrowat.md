---
title: "UIKit - UITableView row 클릭 이벤트 처리하기"
date: 2025-11-25 06:46:00 +0900
categories: [Mobile, iOS]
tags: [uikit, code, uitableview, delegate]
---

`UITableView`의 `row` 중 특정 `row`가 클릭됬을 때 이벤트를 처리하기 위해선 `UITableView`의 `Delegate` 메서드를 통해 처리 가능함.

`ViewController.swift`
```swift
// MARK: - 테이블뷰 Delegate
extension ViewController: UITableViewDelegate{
    
    ...

    // row가 클릭 됬을 때 실행됨
    func tableView(_ tableView: UITableView, didSelectRowAt indexPath: IndexPath) {
      
        let vc = TodoDetailViewController()
        
        self.navigationController?.pushViewController(vc, animated: true)
    }
}
```

결과

![image](/assets/img/uitableviewdidselectrowatresult.gif)

## **Reference**
- [https://velog.io/@gyeomsony/swift-UITableViewDelegate-%ED%99%9C%EC%9A%A9-%EC%98%88%EC%8B%9C](https://velog.io/@gyeomsony/swift-UITableViewDelegate-%ED%99%9C%EC%9A%A9-%EC%98%88%EC%8B%9C)
