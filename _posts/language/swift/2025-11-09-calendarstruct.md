---
title: "Swift - Calendar 구조체"
date: 2025-11-09 08:43:00 +0900
categories: [Language, Swift]
tags: [swift, date, utc, timezone, calendar]
---

`Date` 구조체는 시간대 정보가 없는 특정 시점만을 나타내기 때문에, 이 데이터를 요소로 해석하거나 지역 시간대로 변환하기 위해서는 `Calendar` 구조체가 필요함.

사용 예시
```swift
//var calendar = Calendar.autoupdatingCurrent 사용자가 선택한 달력 기준 ( 글로벌 서비스에서 사용됨. )

var calendar = Calendar.current // Calendar 구조체 인스턴스 생성

calendar.timeZone = TimeZone(identifier: "Asia/Seoul")! // 타임존 설정
calendar.locale = Locale(identifier: "ko_KR") // 지역 설정

let now = Date()

let year = calendar.component(.year, from: now) // 연도 추출
print("now : \(now), year : \(year)")

// 요일 추출
// 일요일: 1
// 월요일: 2
// ...
let weekday = calendar.component(.weekday, from: now)
print(weekday)
```

결과
```
now : 2025-11-08 23:37:02 +0000, year : 2025
1
```

`dateComponents` 메서드를 통해서 여러 개의 요소를 한번에 추출하는 것도 가능함.

사용 예시
```swift
let now = Date()

// DateComponent 타입으로 변환시킴
let dateComponent = calendar.dateComponents([.year, .month, .day], from: now)

print("year : \(dateComponent.year!)")
print("month : \(dateComponent.month!)")
print("day : \(dateComponent.day!)")
```

결과
```
year : 2025
month : 11
day : 9
```

`Calendar` 구조체를 통해 날짜 간에 차이를 구할 수도 있음.

사용 예시
```swift
let startDate = Date()
let endDate = startDate.addingTimeInterval(3600 * 24 * 14) // 14일 후

// 날짜 간에 차이 구하기
let day = calendar.dateComponents([.day], from: startDate, to: endDate).day!
print(day)
```

결과
```
14
```