---
title: "UIKit - TableView 구현 (스토리보드)"
date: 2025-11-01 16:00:00 +0900
categories: [Mobile, iOS]
tags: [uikit, storyboard, tableview]
---

## **TableView 구현 (스토리보드)**

**1.`ViewController`에 `Table View` 요소 올리기**

![image](/assets/img/tableview1.png)

**2.`Table View` 위에 `Table View Cell` 올리기**

![image](/assets/img/tableview2.png)

**3.`Table View Cell`에 필요한 요소 설정**

![image](/assets/img/tableview3.png)

4.`Table View Cell`과 연결할 `UITableViewCell`을 상속받은 클래스 생성

```swift
import UIKit

class TodoTableViewCell: UITableViewCell {
    
    override func setSelected(_ selected: Bool, animated: Bool) {
        super.setSelected(selected, animated: animated)
    }

}
```

**5.`Table View Cell`과 연결**

![image](/assets/img/tableview4.png)

**6.`IBOutlet`으로 `Table View Cell` 위에 있는 요소들 연결**

```swift
import UIKit

class TodoTableViewCell: UITableViewCell {

    @IBOutlet weak var titleLabel: UILabel!
    @IBOutlet weak var detailLabel: UILabel!
    
    override func setSelected(_ selected: Bool, animated: Bool) {
        super.setSelected(selected, animated: animated)
    }

}
```

**7.`Table View Cell Identifier` 설정**

![image](/assets/img/tableview5.png)

**8.`ViewController`에 필요한 로직 작성**
```swift
import UIKit

class ViewController: UIViewController {

    @IBOutlet weak var tableView: UITableView!
    
    var todos: Array<Todo> = [
        Todo(title: "밥 먹기", detail: "라면이랑 먹어야함."),
        Todo(title: "약 먹기", detail: "점심 먹은 후에 약 먹어야함."),
        Todo(title: "공책 사기", detail: "영어 해석 필기 노트 다 떨어져서 공책 사러가야함.")
    ]
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        // 테이블뷰 datasource 설정
        tableView.dataSource = self
        
        // Cell 높이 설정
        tableView.rowHeight = 160
    }
}

extension ViewController: UITableViewDataSource{
    
    // Cell 수
    func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        return todos.count
    }
    
    // 표시될 Cell 구성
    func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        
        // 테이블뷰에서 Cell 꺼내기
        let cell = tableView.dequeueReusableCell(withIdentifier: "todoCell", for: indexPath) as! TodoTableViewCell
        
        let todo = todos[indexPath.row]
        
        cell.titleLabel.text = todo.title
        cell.detailLabel.text = todo.detail
        
        return cell
    }
}
```

> `UITableViewDataSource`는 테이블뷰를 사용하는데 반드시 구현해야하는 프로토콜로, `Cell`의 수, `Cell` 구성에 대한 메서드를 필수적으로 오버라이딩해야함. 추가로, 테이블뷰에 요소가 터치, 스크롤과 같은 동작을 처리하기 위해선 `UITableViewDelegate` 프로토콜을 구현해서 처리가 가능함.
{: .prompt-tip }

결과

![image](/assets/img/tableviewresult.gif)