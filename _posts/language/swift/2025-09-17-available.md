---
title: "Swift - #available이란?"
date: 2025-09-17 19:33:00 +0900
categories: [Language, Swift]
tags: [swift, available, plaform, version]
---

## **#available**
- 해당 기기의 플랫폼 버전을 검증할 때 사용함.
  - 주로 `if`문과 같은 조건문과 혼용해서 사용

문법
```swift
if #available(platform_name version, *){
  // 실행 코드
}else{
  ...
}
```

- 플랫폼 이름과 버전은 띄어쓰기로 구분함.
- 구문을 종료할 경우 마지막에 `*`를 붙여줌.

예시
```swift
if #available(iOS 13, *){
  print("iOS 13 버전 이상임")
}else{
  print("iOS 13 버전 미만임")
}
```

결과
```
iOS 13 버전 이상임
```