---
title: "UIKit - URLSession, @escaping 활용 방법"
date: 2025-11-04 18:34:00 +0900
categories: [Mobile, iOS]
tags: [uikit, code, urlsession, escaping, jsondecoder, jsonencoder, closure]
---

`URLSession`을 통해 네트워킹을 처리할 때 `dataTask` 메서드를 사용해서 처리할 작업을 정의함.

이때, `dataTask` 메서드는 비동기적으로 실행되기 때문에 요청 성공/실패 결과를 그대로 반환할 수 없음.

```swift
session.dataTask(with: request) { data, response, error in
  if let error = error{
      return nil // 불가능함.
  }

  if let data = data{
      guard let result = try? JSONDecoder().decode(ExampleResult.self, from: data) else{
          return nil // 불가능함.
      }
      
      return result // 불가능함.
  }
}.resume()
```

이런 이유 때문에 `@escaping`을 통해서 비동기 처리가 끝났을 때 전달받은 클로저를 실행하여 처리함.

구현 예시

```swift
import UIKit

// MARK: - 응답 객체
struct ResponseWrapper<T: Codable>: Codable {
    let status: Int
    let success: Bool
    let message: String
    let result: T
    let timestamp: String
}

struct RegisterResult: Codable {
    let userSeq: Int
    let email, role, nickname, provider: String
    let createdAt, modifiedAt: String
}

// MARK: - 요청 객체
struct RegisterRequest: Codable{
    let email: String
    let password: String
    let nickname: String
}

// MARK: - 네트워킹 처리 서비스
final class AuthService{
    
    // URLSession 객체 생성
    let session = URLSession.shared
    
    func fetchRegister(data: RegisterRequest, completion: @escaping (RegisterResult?) -> Void){
        
        // URLRequest 생성
        var request = URLRequest(url: URL(string: "http://localhost:9000/api/v1/auth/register")!)
        
        // 매개변수로 전달된 RegisterRequest 직렬화
        let body = try? JSONEncoder().encode(data)
        
        // HTTP Method, Header, Body 설정
        request.httpMethod = "POST"
        request.addValue("application/json", forHTTPHeaderField: "Content-Type")
        request.httpBody = body
        
        // 네트워킹 작업 정의
        let task = session.dataTask(with: request) { data, response, error in
            // dataTask 메서드가 비동기로 실행되기 때문에 응답받은 데이터를 바로 리턴할 수 없음.
            // 따라서 클로저를 매개 변수로 받은 후 요청이 성공하거나 실패했을 때 호출하여 콜백 함수 구조로 처리함.
            
            if let error = error{
                completion(nil)
                return
            }
            
            if let data = data{
                // 응답 데이터 역직렬화
                guard let result = try? JSONDecoder().decode(ResponseWrapper<RegisterResult>.self, from: data) else{
                    completion(nil)
                    return
                }
                
                // 콜백 함수 호출
                completion(result.result)
            }
        }
        
        task.resume() // 작업 시작
    }
}

// MARK: - 테스트용 ViewController
final class DummyViewController{
    private let authService = AuthService()
    
    func register(request: RegisterRequest){
        authService.fetchRegister(data: request) { result in
            dump(result)
        }
    }
}

// MARK: - 테스트
let request = RegisterRequest(email: "test2@gmail.com", password: "test1234!", nickname: "test2gmail")

let vc = DummyViewController()

vc.register(request: request)
```

결과
```
...
    - userSeq: 15
    - email: "test2@gmail.com"
    - role: "ROLE_USER"
    - nickname: "test2gmail"
    - provider: "LIBRONOTE"
    - createdAt: "2025-11-04T09:24:29"
    - modifiedAt: "2025-11-04T09:24:29"
```