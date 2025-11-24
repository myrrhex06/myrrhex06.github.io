---
title: "UIKit - CoreData fetch uuid 조건 설정하기"
date: 2025-11-24 19:08:00 +0900
categories: [Mobile, iOS]
tags: [uikit, code, coredata, predicate, context, request]
---

`CoreDataManager.swift`
```swift
import UIKit
import CoreData

final class CoreDataManager{

    ...

    func deleteTodo(uuid: UUID){
        let request = Todo.fetchRequest()

        // UUID 매칭 조건식
        request.predicate = NSPredicate(format: "%K == %@", "todoId", uuid as CVarArg)
        
        let todos = try? context.fetch(request)
        
        guard let todo = todos?.first else { return }

        context.delete(todo)
        try? context.save()
    }

    ...
}
```

## **Reference**
- [https://stackoverflow.com/questions/68412063/swift-5-coredata-predicate-using-uuid](https://stackoverflow.com/questions/68412063/swift-5-coredata-predicate-using-uuid)