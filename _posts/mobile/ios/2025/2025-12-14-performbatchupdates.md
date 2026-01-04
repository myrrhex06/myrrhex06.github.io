---
title: "UIKit - performBatchUpdates 메서드"
date: 2025-12-14 21:01:00 +0900
categories: [Mobile, iOS]
tags: [uikit, uitableview, uicollectionview, performbatchupdates, deleterows, insertrows]
---

![image](/assets/img/uitableviewperformbatchupdatesdocumentation.png)

`UITableView`, `UICollectionView`에서 여러 변경 사항들(`delete`, `insert`, `reload`, `move` 등)을 하나의 트랜잭션으로 묶어 데이터 소스와 화면 상태의 불일치를 방지할 수 있게 해줌.

> Apple은 `beginUpdates`, `endUpdates` 메서드보다 `performBatchUpdates` 메서드 사용을 권고하고 있음.
{: .prompt-tip }

![image](/assets/img/performbatchupdatesparameters.png)

- `updates`: `insert`, `delete`, `reload`, `move` 등의 변경 사항들을 묶어서 처리하는 `block`
- `completion`: 모든 변경 사항의 애니메이션이 처리된 후 호출되는 클로저

`performBatchUpdates` 메서드가 실행될 때는 항상 `delete`를 먼저 처리한 후에 `insert`를 처리함.

> `insert` 코드를 먼저 작성하더라도 `delete`가 우선적으로 실행됨.
{: .prompt-tip }

사용 예시
```swift
extension ViewController: UITableViewDelegate{
    
    func tableView(_ tableView: UITableView, trailingSwipeActionsConfigurationForRowAt indexPath: IndexPath) -> UISwipeActionsConfiguration? {
 
        ...

            // performBatchUpdates 메서드 실행 전 데이터 소스를 먼저 업데이트함
            self.todoManager.setTodoList(todoList: todos)

            self.tableView.performBatchUpdates {
                self.tableView.deleteRows(at: [indexPath], with: .fade)
                if isInsert{
                    self.tableView.insertRows(at: [IndexPath(row: todos.count - 1, section: 0)], with: .none)
                }
            }
        }
        
        ...
    }
```

## **Reference**

- [https://developer.apple.com/documentation/uikit/uitableview/performbatchupdates(_:completion:)/](https://developer.apple.com/documentation/uikit/uitableview/performbatchupdates(_:completion:)/)
- [https://skytitan.tistory.com/516](https://skytitan.tistory.com/516)