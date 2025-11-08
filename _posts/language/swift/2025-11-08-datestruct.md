---
title: "Swift - Date 구조체"
date: 2025-11-08 13:30:00 +0900
categories: [Language, Swift]
tags: [swift, date, utc, timezone]
---

Swift에서 날짜를 다루는 가장 기본적인 구조체임.

`Date`는 `Timezone` 정보가 없는 특정 시점 그대로를 나타내며, 콘솔에 출력하는 것과 같은 상황에서는 표준 시각(UTC)로 표시됨.

예시
```swift
import Foundation

let now = Date() // Date 구조체 인스턴스는 현재 시점을 기준으로 날짜 데이터를 만듬
print(now)

// 어제 날짜 구하기
let yesterday = now - 86400 // 86400초 = 24시간을 의미함
print(yesterday)
```

출력
```
2025-11-08 04:22:33 +0000
2025-11-07 04:22:33 +0000
```

> `Date`는 단순한 시간 데이터일 뿐이고, 실제로 우리가 보고 싶은 형태로 바꾸려면 `Calendar`나 `DateFormatter`를 이용해야 함.
{: .prompt-tip }