---
title: "UIKIt - URLComponents를 활용하여 URL 생성하기"
date: 2026-03-19 06:45:00 +0900
categories: [Mobile, iOS]
tags: [ios, uikit, urlsession, urlcomponent, url]
---

기존에는 `URL` 객체를 생성할 때 아래와 같이 생성했음.

```swift
func list(page: Int, size: Int, title: String? = nil, userSeq: String? = nil, completionHandler: @escaping (Result<PagingResponse<[BookListResponse]>, NetworkingError>) -> Void){
        
        // URL 객체 생성
        guard let url = URL(string: "\(Constants.API_URL)/books?page=\(page)&size=\(size)&title=\(title != nil ? title! : "")&userSeq=\(userSeq != nil ? userSeq! : "")") else { return }
        
        var request = URLRequest(url: url)
        
        urlSession.dataTask(with: request){ data, response, error in

            ...

        }
}
```

이렇게 생성하다보니 쿼리 파라미터를 붙이는데 굉장히 번거로웠고 이를 해결하기 위해서 `URLComponents`를 사용함.

`URLComponents`란 `URL` 객체를 구성하는 구조임. URL 객체는 read-only 이지만 `URLComponents`는 read, write 둘 다 가능함.

예시
```swift
func list(page: Int, size: Int, title: String? = nil, userSeq: String? = nil, completionHandler: @escaping (Result<PagingResponse<[BookListResponse]>, NetworkingError>) -> Void){
        
        // URLComponents 객체 생성
        guard var component = URLComponents(string: "\(Constants.API_URL)/books") else { return }
        
        // 쿼리 스트링 추가
        component.queryItems = [
            URLQueryItem(name: "page", value: "\(page)"),
            URLQueryItem(name: "size", value: "\(size)"),
            URLQueryItem(name: "title", value: title),
            URLQueryItem(name: "userSeq", value: userSeq)
        ]

        // URL 생성
        guard let url = component.url else { return }
        
        var request = URLRequest(url: url)
        
        urlSession.dataTask(with: request){ data, response, error in

            ...

        }
}
```