---
title: "Swift - Result 타입 Void 반환 방법"
date: 2026-03-17 06:33:00 +0900
categories: [Language, Swift]
tags: [swift, void, result]
---

`Result` 타입을 사용해서 처리할 때 `success` 값으로 `Void`를 반환해야하는 경우 아래와 같이 빈 튜플을 넘겨 처리 가능함.

예시
```swift
final class AuthService{
    func logout(completionHandler: @escaping (Result<Void, NetworkingError>) -> Void){
        
        guard let url = URL(string: "\(Constants.API_URL)/auth/logout") else { return }
        
        var request = URLRequest(url: url)
        ...
        
        urlSession.dataTask(with: request) { data, response, error in

            ....
            
            // 빈 튜플 넘기기
            completionHandler(Result.success(()))
        }.resume()
    }
}
```