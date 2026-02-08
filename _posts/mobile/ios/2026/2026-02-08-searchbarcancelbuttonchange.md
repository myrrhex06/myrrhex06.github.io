---
title: "UIKit - SearchController cancel 버튼 텍스트 변경하기"
date: 2026-02-08 13:37:00 +0900
categories: [Mobile, iOS]
tags: [ios, uikit, searchcontroller, forkey]
---

아래와 같이 `SearchController`를 클릭하면 취소 버튼에 `Cancel` 문구가 표시됨.

![image](/assets/img/beforechangingcanceltext.gif)

이 문구를 다른 문구로 바꾸기 위해서는 아래와 같이 코드를 작성하면 됨.

```swift
import UIKit

final class BookListViewController: UIViewController {
    
    private let searchController = UISearchController()

    private func setupSearchController(){
        ...
        
        // Cancel 버튼 문구 변경
        searchController.searchBar.setValue("취소", forKey: "cancelButtonText")
        
        self.navigationItem.searchController = searchController
    }

}
```

결과

![image](/assets/img/afterchangingcanceltext.gif)

## **Reference**
- [https://minwoostory.tistory.com/159](https://minwoostory.tistory.com/159)