---
title: "Swift - 입력 처리 함수"
date: 2025-10-05 08:25:00 +0900
categories: [Language, Swift]
tags: [swift, readLine, optional]
---

백준 문제를 풀어보려고 했으나, 생각해보니 Swift에서 입력을 처리하는 함수를 따로 본적이 없어서 이렇게 정리함.

## **입력 처리 함수**
콘솔에서 사용자의 입력을 받아 처리하는 함수는 `readLine`임.

> 플레이그라운드에서는 실행할 수 없으며, Command Line Tool 프로젝트에서 사용 가능함.
{: .prompt-tip }

예시
```swift
let result = readLine()
print(result)
```

결과
```
1 3
Optional("1 3")
```

위 예시처럼 결과값은 옵셔널 타입으로 반환되며, 입력이 반드시 존재하는 상황이라면 강제 해제 연산자(`!`)를 통해 간편하게 처리가 가능함.

만약 띄워쓰기를 기준으로 입력 값을 갈라야한다면 `split`을 통해 처리가 가능함.

예시
```swift
let result = readLine()!.split(separator: " ")
print(result)
```

결과
```
1 3
["1", "3"]
```