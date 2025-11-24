---
title: "UIKit - UITableView row 삭제 처리하기"
date: 2025-11-24 06:49:00 +0900
categories: [Mobile, iOS]
tags: [uikit, code, uitableview]
---

테이블뷰의 요소가 삭제되었을 때 기존에는 `reloadData()` 메서드를 통해 테이블뷰를 다시 그려줬음.

![image](/assets/img/tableviewreloaddatafunctionuseresult.gif)

위 화면처럼 다시 그리는 과정에서 뚝뚝 끊기는 현상이 발생함.

`reloadData()` 메서드는 `row` 뿐만 아니라 테이블뷰의 모든 요소를 다시 로드하기 때문에 위와 같은 문제가 발생하게됨.

이런 문제는 `deleteRows()` 메서드를 통해 삭제될 `row`를 지정하여 매끄럽게 화면이 구성되도록 처리할 수 있음.

> `delete` 외에도 `insert`, `reload`를 `row` 단위로 처리하는 메서드들이 존재함.
{: .prompt-tip }

이 메서드를 사용하기 위해선 `beginUpdates()` 메서드를 통해 테이블뷰 요소가 변경될 것이라는 것을 알린 후에 `deleteRows()` 메서드를 호출해야하며, 마지막에는 `endUpdates()` 메서드를 호출해서 테이블뷰 업데이트가 끝났다는 걸 알려야함.

`ViewController.swift`
```swift

...

// MARK: - 테이블뷰 Delegate
extension ViewController: UITableViewDelegate{
    
    func tableView(_ tableView: UITableView, trailingSwipeActionsConfigurationForRowAt indexPath: IndexPath) -> UISwipeActionsConfiguration? {
        
        let deleteButton = UIContextualAction(style: .destructive, title: nil) { _, _, success in
            
            ...

            // row 제거 전 먼저 테이블 뷰에 표시되는 배열에서 요소를 제거함
            self.coreDataManager.deleteTodo(uuid: todo.todoId!)
            
            // TableView 업데이트 시작
            self.tableView.beginUpdates()

            // row 제거
            self.tableView.deleteRows(at: [indexPath], with: .fade)

            // TableView 업데이트 종료
            self.tableView.endUpdates()
            
            ...
        }
        
        deleteButton.image = UIImage(systemName: "trash")
        
        return UISwipeActionsConfiguration(actions: [deleteButton])
    }
}
```

> `deleteRows` 메서드는 단순히 테이뷸뷰에 표시되는 `row`만 없애는 역할이기 때문에 테이블뷰를 표시하는 데이터 배열에서 먼저 데이터를 지운 후에 사용해야함.
{: .prompt-tip }

결과

![image](/assets/img/tableviewdeleterowuseresult.gif)

## **Reference**
- [https://yy-dev.tistory.com/95](https://yy-dev.tistory.com/95)