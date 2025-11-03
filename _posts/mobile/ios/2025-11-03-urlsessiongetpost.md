---
title: "UIKit - URLSession을 통해 GET, POST 요청 보내기"
date: 2025-11-03 20:50:00 +0900
categories: [Mobile, iOS]
tags: [uikit, code, urlsession, networking, jsondecoder]
---

## **URLSession을 통해 GET, POST 요청 보내기**
앱 상에서 서버로 요청을 보낼 때는 기본적으로 `URLSession`을 통해 처리함.

> 아래 예시에서 사용하는 API들은 사이드 프로젝트로 만들어둔 백엔드 API를 재사용함.

### **GET 요청**

```swift
import UIKit

// URL 객체 생성
let url = URL(string: "http://localhost:9000/api/v1/books?page=0&size=10")!

// URLSession 생성 및 작업 정의
URLSession.shared.dataTask(with: url) { data, response, error in
    if let error = error{
        print(error)
        return
    }
    
    if let data = data {
        print(String(decoding: data, as: UTF8.self))
        return
    }
}.resume() // 작업 실행
```

결과
```
{"status":200,"success":true,"message":"success","result":[{"bookSeq":2,"title":"책제목1","nickname":"test","isbn":"ISBN1","fileSeq":null,"createdAt":"2025-08-16T07:07:44","modifiedAt":"2025-08-16T07:07:44"},{"bookSeq":3,"title":"책제목","nickname":"test","isbn":"ISBN번호","fileSeq":null,"createdAt":"2025-08-16T13:44:02","modifiedAt":"2025-08-16T13:44:02"}],"timestamp":"2025-11-03T20:48:24.811422"}
```

### **POST 요청**
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

let registerRequest = RegisterRequest(email: "test123@gmail.com", password: "test1234!", nickname: "테스트유저")

do{
    // URL 객체 생성
    let url = URL(string: "http://localhost:9000/api/v1/auth/register")!
    
    // JSON Encoder 생성
    let encoder = JSONEncoder()
    
    // Swift 구조체 직렬화 (JSON 문자열로 변환)
    let json = try encoder.encode(registerRequest)

    // URLRequest 생성
    var request = URLRequest(url: url)
    
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
            print(String(decoding: data, as: UTF8.self))
        }
    }.resume() // 작업 실행
    
}catch{
    print(error)
}
```

결과
```
{"status":200,"success":true,"message":"success","result":{"userSeq":10,"email":"test123@gmail.com","role":"ROLE_USER","nickname":"테스트유저","provider":"LIBRONOTE","createdAt":"2025-11-03T11:35:25","modifiedAt":"2025-11-03T11:35:25"},"timestamp":"2025-11-03T20:35:25.026847"}
```