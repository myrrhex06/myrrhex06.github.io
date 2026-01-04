---
title: "UIKit - CoreData 정렬 적용하기"
date: 2025-12-06 11:30:00 +0900
categories: [Mobile, iOS]
tags: [uikit, coredata, sort, fetch]
---

데이터를 조회할 때 정렬을 적용하기 위해선 `NSSortDescriptor`를 통해 정렬 조건을 설정하여 `NSFetchRequest`에 적용하는 것으로 처리할 수 있음.

`CoreDataManager.swift`
```swift
import UIKit
import CoreData

final class CoreDataManager{
    func fetchTodoList() {
        let request = Todo.fetchRequest()

        // 정렬을 적용시킬 프로퍼티명과 오름차순(ascending) 여부 설정
        let sort = NSSortDescriptor(key: "createdDate", ascending: false)
        
        // 정렬 조건 적용
        request.sortDescriptors = [sort]
        
        do{
            let data = try context.fetch(request)
            todoList = data
        }catch{
            print("에러 발생 \(error)")
        }
    }
}
```

## **Reference**
- [https://velog.io/@minsang/CoreData%EC%9E%AC%EC%9D%80%EC%94%A8#nssortdescriptor](https://velog.io/@minsang/CoreData%EC%9E%AC%EC%9D%80%EC%94%A8#nssortdescriptor)