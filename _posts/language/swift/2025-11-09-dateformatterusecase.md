---
title: "Swift - DateFormatter란?"
date: 2025-11-09 09:10:00 +0900
categories: [Language, Swift]
tags: [swift, date, utc, timezone, dateformatter]
---

`Date` 구조체로 생성한 데이터를 문자열로 변환하기 위해서 `DateFormatter`를 사용함.

사용 예시
```swift
import UIKit

// DateFormatter 인스턴스 생성
let formatter = DateFormatter()

formatter.timeZone = TimeZone.current // 타임존 설정
formatter.locale = Locale(identifier: "ko_KR") // 지역 설정

formatter.dateStyle = .short // 날짜 표시 스타일 지정
formatter.timeStyle = .short // 시간 표시 스타일 지정

// Date 타입을 문자열로 변환
let dateStr = formatter.string(from: Date())
print(dateStr)
```

결과
```
2025. 11. 9. 오전 9:08
```

날짜와 시간을 표시하는 스타일들을 미리 정의된 열거형 값을 통해 설정할 수 있음.

미리 정의된 스타일 외에도 직접 커스텀 형식을 설정하여 변환하는 것도 가능함.

사용 예시
```swift
import UIKit

// DateFormatter 인스턴스 생성
let formatter = DateFormatter()

formatter.timeZone = TimeZone.current // 타임존 설정
formatter.locale = Locale(identifier: "ko_KR") // 지역 설정

// 커스텀 형식 설정
formatter.dateFormat = "yyyy-MM-dd HH:mm:ss"

// 문자열 변환
let dateStr2 = formatter.string(from: Date())
print(dateStr2)
```

결과
```
2025-11-09 09:09:18
```

`DateFormatter`에 설정된 형식에 맞춰 `Date` 타입 값으로 변환하는 것도 가능함.

예시
```swift
import UIKit

let formatter = DateFormatter()

formatter.timeZone = TimeZone.current // 타임존 설정
formatter.locale = Locale(identifier: "ko_KR") // 지역 설정

// 커스텀 형식 설정
formatter.dateFormat = "yyyy-MM-dd HH:mm:ss"

// 지정된 형식에 맞는 문자열을 Date로 변환 가능
let date = formatter.date(from: "2025-11-09 08:59:27")
print(date!)
```

결과
```
2025-11-08 23:59:27 +0000
```