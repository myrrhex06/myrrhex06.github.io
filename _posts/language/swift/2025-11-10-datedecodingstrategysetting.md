---
title: "Swift - JSONDecoder Date 타입 처리"
date: 2025-11-10 20:20:00 +0900
categories: [Language, Swift]
tags: [swift, json, date, jsondecoder]
---

HTTP 요청을 통해 문자열 형태의 날짜 데이터를 얻은 후에 Swift 객체로 변환하기 위해 `JSONDecoder`를 사용할 때 `dateDecodingStrategy` 프로퍼티를 통해 날짜 포맷을 지정해줘야함.

`Date` 구조체로 변환할 때 날짜 포맷을 지정해주는 이유는 문자열 형태의 날짜 포맷이 다양하기 때문에 개발자가 명시적으로 지정하지 않으면 `JSONDecoder`가 알 수 없어서 객체 변환에 실패하기 때문임.

사용 예시
```swift
...

let decoder = JSONDecoder()
decoder.dateDecodingStrategy = .iso8601 // 날짜 포맷 지정

let decoded = try? decoder.decode(MusicData.self, from: data)

...
```

> `iso8601`는 날짜와 시간을 표현하는 국제 표준 형식임.
{: .prompt-tip }