---
title: "Troubleshooting - UITableView 무한 스크롤 기능을 적용시킨 상태에서 deleteRows 메서드 처리 시 데이터 불일치 문제 해결"
date: 2025-12-14 20:31:00 +0900
categories: [Troubleshooting]
tags: [uikit, tableview, coredata, deleteRows, insertRows]
---

## **이슈**

전체 데이터는 아래 화면과 같음.

![image](/assets/img/originaltableviewdata.gif)

시뮬레이터를 재시작한 후 스크롤을 하지 않은 상태에서 요소를 삭제할 경우 아래와 같이 데이터가 추가적으로 채워지지 않는 현상이 발생함.

![image](/assets/img/deleterowsuitableviewdata.gif)

구현된 코드는 아래와 같음.
```swift
extension ViewController: UITableViewDelegate{
    
    func tableView(_ tableView: UITableView, trailingSwipeActionsConfigurationForRowAt indexPath: IndexPath) -> UISwipeActionsConfiguration? {
        
        let deleteButton = UIContextualAction(style: .destructive, title: nil) { _, _, success in
            
            let todos = self.todoManager.getTodoList()
            
            let todo = todos[indexPath.row]
            
            guard let uuid = todo.todoId else { return }
            
            self.todoManager.deleteTodo(uuid: uuid)
            var todoList = self.todoManager.getTodoList()
            todoList.remove(at: indexPath.row)
            self.todoManager.setTodoList(todoList: todoList)
            
            self.tableView.beginUpdates()
            self.tableView.deleteRows(at: [indexPath], with: .fade)
            self.tableView.endUpdates()
            
            success(true)
        }
        
        deleteButton.image = UIImage(systemName: "trash")
        
        return UISwipeActionsConfiguration(actions: [deleteButton])
    }

}
```

## **원인**

무한 스크롤 기능을 구현하기 위해 작성한 코드는 아래와 같음.

```swift
// MARK: - 테이블뷰 Delegate
extension ViewController: UITableViewDelegate{

    func scrollViewDidScroll(_ scrollView: UIScrollView) {
        if !todoManager.getIsDataEnded(){
            
            let offsetY = scrollView.contentOffset.y
            let height = scrollView.contentSize.height
            
            if offsetY > height - scrollView.bounds.height {
                
                let limit = todoManager.getLimit()
                let offset = todoManager.getOffset()
                
                let nextOffset = offset + limit
                
                todoManager.setOffset(offset: nextOffset)
                todoManager.setLimit(limit: limit)
                
                guard let todos = todoManager.fetchTodos() else { return }
                
                if todos.isEmpty {
                    
                    todoManager.setLimit(limit: limit)
                    todoManager.setOffset(offset: offset)
                    
                    todoManager.setIsDataEnded(isDataEnded: true)
                    
                    return
                }
                
                todoManager.appendTodoList(todos: todos)
                
                tableView.reloadData()
                
                todoManager.setIsDataEnded(isDataEnded: false)
            }
        }
    }
}
```

스크롤을하면 설정되어 있던 `offset`에 `limit`이 더해져 바로 다음 데이터를 가져올 수 있도록 구현하였는데, 이것이 문제였음.

`offset`과 `limit`의 초기값은 각각 0, 10으로 설정되어 있음.

총 데이터의 개수가 12라고 가정하고, 여기서 3개를 지운다고 하면 데이터의 개수는 9개가 됨.

스크롤을 하면 `offset` 값에 `limit` 값이 더해지게 되고 그럼 `offset`은 10으로 설정되어 10개의 데이터를 건너뛴 다음 데이터를 찾게 됨.

![image](/assets/img/deleterowsuitableviewdata.gif)

스크롤을 하더라도 10개를 건너 뛰고 데이터를 조회하려고 하기 때문에 데이터가 위 화면처럼 제대로 표시되지 않은 것이였음.

## **해결**

`delete`를 한 후에 `fetch`를 한번 더 하여 기존 배열의 데이터와 불일치하는지 검증 한 후에 불일치하다면 요소를 추출하여 배열에 `append`한 후 `insertRows`까지 처리하도록 코드를 수정하여 해결함.

```swift
extension ViewController: UITableViewDelegate{
    
    func tableView(_ tableView: UITableView, trailingSwipeActionsConfigurationForRowAt indexPath: IndexPath) -> UISwipeActionsConfiguration? {
        
        let deleteButton = UIContextualAction(style: .destructive, title: nil) { _, _, success in
        // insertRows 여부 판단을 위한 플래그 변수
            var isInsert: Bool = false
            
            // 원본 할일 배열 조회
            var todos = self.todoManager.getTodoList()
            
            let todo = todos[indexPath.row]
            
            guard let uuid = todo.todoId else { return }
            
            // CoreData, 배열 요소 삭제
            self.todoManager.deleteTodo(uuid: uuid)
            todos.remove(at: indexPath.row)
            
            // 데이터 fetch
            guard let fetchTodoList = self.todoManager.fetchTodos() else { return }
            
            // 원본 데이터 배열에 포함되는지 여부 검증
            if !todos.contains(fetchTodoList){
              
              // 포함되지 않는 데이터 추출(스와이프 처리를 했기 때문에 추출되는 데이터 수는 1개임)
                let todo = fetchTodoList.filter {
                    !todos.contains($0)
                }
                
                // 원본 데이터 배열에 append
                todos.append(contentsOf: todo)

                isInsert = true
            }else{
                isInsert = false
            }
            
            // 데이터 배열 설정
            self.todoManager.setTodoList(todoList: todos)
            
            self.tableView.beginUpdates()
            // UITableView row 제거
            self.tableView.deleteRows(at: [indexPath], with: .fade)
            if isInsert{
              // UITableView row 추가
                self.tableView.insertRows(at: [IndexPath(row: todos.count - 1, section: 0)], with: .none)
            }
            self.tableView.endUpdates()
            
            success(true)
        }
        
        deleteButton.image = UIImage(systemName: "trash")
        
        return UISwipeActionsConfiguration(actions: [deleteButton])
    }
}
```

결과

![image](/assets/img/deleterowsuitableviewdataresolveresult.gif)