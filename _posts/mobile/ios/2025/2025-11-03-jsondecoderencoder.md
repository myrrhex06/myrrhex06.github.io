---
title: "UIKit - JSONDecoder, JSONEncoder를 통해서 JSON 문자열 직렬화 및 역직렬화 처리하기"
date: 2025-11-03 21:41:00 +0900
categories: [Mobile, iOS]
tags: [uikit, code, jsonencoder, jsondecoder, http, networking, urlsession]
---

## **JSONDecoder, JSONEncoder를 통해서 JSON 문자열 직렬화 및 역직렬화 처리하기**

- **`JSONDecoder`**: `JSON` 문자열을 Swift 구조체 또는 클래스로 변환할 때 사용.
  - 변환되는 구조체 또는 클래스는 `Decodable` 또는 `Codable` 프로토콜을 구현하고 있어야만함.
- **`JSONEncoder`**: Swift 구조체 또는 클래스를 `JSON` 문자열로 변환할 때 사용.
  - 변환되는 구조체 또는 클래스는 `Encodable` 또는 `Codable` 프로토콜을 구현하고 있어야만함.

> `Codable` 프로토콜은 `Decodable`, `Encodable`을 모두 포함하고 있는 프로토콜임.
{: .prompt-tip }

사용 예시
```swift
import UIKit

// API에 정의되어 있는 Request
// Codable 프로토콜: 객체 직렬화, 역직렬화를 위해 구현해줘야함.
struct RegisterRequest: Codable{
    let email: String
    let password: String
    let nickname: String
}

// API 응답 래퍼
struct ResponseWrapper: Codable {
    let status: Int
    let success: Bool
    let message: String
    let result: Result
    let timestamp: String
}

// API 응답 결과
struct Result: Codable {
    let userSeq: Int
    let email, role, nickname, provider: String
    let createdAt, modifiedAt: String
}

let registerRequest = RegisterRequest(email: "test123456@gmail.com", password: "test1234!", nickname: "테스트유저100")

func encodeJson(request: RegisterRequest) -> Data?{
    do{
        // JSON Encoder 생성
        let encoder = JSONEncoder()
        
        // Swift 구조체 직렬화 (JSON 문자열로 변환)
        let json = try encoder.encode(request)
        
        return json
    }catch{
        print(error)
        return nil
    }
}

func decodeJson(data: Data) -> Result? {
    do{
        // JSON Decoder 생성
        let decoder = JSONDecoder()
        
        // JSON 문자열 역직렬화 (Swift 객체로 변환)
        let decodedData = try decoder.decode(ResponseWrapper.self, from: data)
        
        return decodedData.result
    }catch{
        print(error)
        return nil
    }
}


// URL 객체 생성
let url = URL(string: "http://localhost:9000/api/v1/auth/register")!

// URLRequest 생성
var request = URLRequest(url: url)

let json = encodeJson(request: registerRequest)!

request.httpMethod = "POST" // HTTP 메서드 설정
request.httpBody = json // HTTP 요청 본문 설정

request.addValue("application/json", forHTTPHeaderField: "Content-Type") // HTTP 헤더 설정

// URLSession 생성 및 작업 정의
URLSession.shared.dataTask(with: request) { data, response, error in
    if let error = error{
        print(error)
        return
    }
    
    if let data = data{
        let result = decodeJson(data: data)
        
        if let result = result {
            print(result)
        }
    }
}.resume() // 작업 실행
```

결과
```
Result(userSeq: 13, email: "test123456@gmail.com", role: "ROLE_USER", nickname: "테스트유저100", provider: "LIBRONOTE", createdAt: "2025-11-03T12:40:47", modifiedAt: "2025-11-03T12:40:47")
```