---
title: "UIKit - UITableView 무한 스크롤 구현하기"
date: 2026-02-17 21:28:00 +0900
categories: [Mobile, iOS]
tags: [ios, uikit, uitableview, uiscrollview, urlsession, paging]
---

`UITableViewDelegate`의 `scrollViewDidScroll` 메서드를 구현하여 `ScrollView` 하단에 내려왔을 때 다음 페이지의 데이터를 불러오도록 구현함.

`BookListViewController.swift`
```swift
extension BookListViewController: UITableViewDataSource, UITableViewDelegate{
    func scrollViewDidScroll(_ scrollView: UIScrollView) {
        let offsetY = scrollView.contentOffset.y
        let contentHeight = scrollView.contentSize.height
        
        if offsetY > contentHeight - scrollView.frame.height {
            bookListManager.fetchNext { result in
                switch result{
                case .success(_):
                    DispatchQueue.main.async {
                        self.tableView.reloadData()
                    }
                case .failure(let error):
                    print("error : \(error)")
                }
            }
        }
    }
}
```

`BookListManager.swift`
```swift
import Foundation

final class BookListManager{
    
    private let bookService: BookService = BookService.shared
    
    private var totalPages: Int = 0
    
    private var page: Int = 0
    
    private var size: Int = 10
    
    private var bookList: [BookListResponse] = []
    
    /// 로딩 여부
    private var isLoading: Bool = false
    
    /// 마지막 페이지 여부 검증
    private var isLastPage: Bool{
        page == totalPages - 1
    }
    
    /// 책 기록 조회
    /// - Parameters:
    ///   - title: 책 제목
    ///   - completionHandler: 처리 결과 반환
    public func fetch(title: String? = nil, completionHandler: @escaping (Result<PagingResponse<[BookListResponse]>, NetworkingError>) -> Void) {
        // 이미 요청이 실행중일 경우 return
        guard !isLoading else { return }
        page = 0
        
        isLoading = true
        bookService.list(page: page, size: size, title: title) { result in
            
            switch result{
            case .success(let response):
                // 전체 페이지 설정
                self.totalPages = response.totalPages
                self.bookList = response.content
                // 로딩 종료
                self.isLoading = false
                
                completionHandler(Result.success(response))
            case .failure(let error):
                // 실패 시 로딩 종료
                self.isLoading = false
                
                completionHandler(Result.failure(error))
            }
        }
    }
    
    
    /// 다음 페이지 책 기록 조회
    /// - Parameters:
    ///   - title: 책 제목
    ///   - completionHandler: 처리 결과 반환
    public func fetchNext(title: String? = nil, completionHandler: @escaping (Result<PagingResponse<[BookListResponse]>, NetworkingError>) -> Void) {
      // 이미 요청 중이거나 마지막 페이지일 경우 return
        guard !isLoading, !isLastPage else { return }
        let nextPage = page + 1
        
        isLoading = true
        bookService.list(page: nextPage, size: size, title: title) { result in
            
            switch result{
            case .success(let response):
                self.page = nextPage
                self.bookList.append(contentsOf: response.content)
                self.isLoading = false
                
                completionHandler(Result.success(response))
            case .failure(let error):
                self.isLoading = false
                
                completionHandler(Result.failure(error))
            }
        }
    }
}
```

결과

![image](/assets/img/tableviewinfinitescrollview.gif)
