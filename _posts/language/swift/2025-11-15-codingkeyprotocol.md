---
title: "Swift - CodingKey 프로토콜이란?"
date: 2025-11-15 15:08:00 +0900
categories: [Language, Swift]
tags: [swift, json, protocol, codingkey, json]
---

`JSON` 형태를 가진 데이터의 `key` 값을 구조체(or 클래스)의 필드명과 매핑시킬 수 있도록 해주는 프로토콜임.

예시
```swift
import Foundation

struct SoftwareData: Codable{
    var resultCount: Int
    var results: [Software]
}

struct Software: Codable{
    
    var imageUrl: String?
    
    enum CodingKeys: String, CodingKey {
        case imageUrl = "artworkUrl100"
    }
}
```

위 예시에서 `Software`라는 구조체는 `imageUrl`이라는 필드를 가지지만 실제 HTTP 응답으로는 `artworkUrl100` 라는 `key` 값으로 넘어옴.

이런 경우 필드명을 맞추기 위해 `CodingKey` 프로토콜을 채택하여 사용함.

`CodingKey` 프로토콜을 채택할 때는 반드시 `String` 타입의 원시값(`rawValue`)를 갖는 열거형(`enum`)이여야함.