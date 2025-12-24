---
title: "xcconfig 파일로 환경 변수 설정하기"
date: 2025-12-24 06:50:00 +0900
categories: [Mobile, iOS]
tags: [xcode, git, xcconfig, cocoapods]
---

## **xcconfig 파일이란?**
환경별 설정 값(`API_KEY`, `BASE_URL` 등)을 관리하기 위한 설정 파일임.

외부 저장소(ex. git)에 노출되면 안되는 값들(ex. API_KEY)을 관리하는 용도로도 많이 사용함.

```
API_KEY = 1212
```

## **생성 및 설정**
프로젝트 루트 경로에서 새 파일 생성하여 아래와 같이 선택해줌.

<img src="/assets/img/xcconfigsetting01.png" alt="image" width="500">

생성된 xcconfig 파일로 접근하여 환경 변수를 설정해줌.

```
#include "Pods/Target Support Files/Pods-Libronote/Pods-Libronote.debug.xcconfig"

BASE_URL = http:/$()/localhost:8080
```

CocoaPods에서 생성한 빌드 설정을 그대로 상속받기 위해 해당 xcconfig 파일을 `include` 해줌.

> xcconfig에서 위와 같이 `BASE_URL`을 명시할 때는 `//`가 주석으로 처리되어 사이에 `$()`을 넣어줘야함.
{: .prompt-tip }

아래와 같이 Project Setting에서 xcconfig 파일을 설정해줌.

<img src="/assets/img/xcconfigsetting02.png" alt="image" width="500">

Info.plist 파일에 아래와 같이 환경 변수를 설정해줌.

<img src="/assets/img/xcconfigsetting03.png" alt="image" width="500">

사용은 아래와 같이할 수 있음.

```swift
private var apiUrl: String{
  guard let apiUrl = Bundle.main.object(forInfoDictionaryKey: "BASE_URL") as? String else{
            fatalError()
  }
        
  return apiUrl
}
```

만약 .gitignore 파일에 xcconfig가 설정되어 있지 않다면 아래와 같이 추가해줘야함.

```
...

*.xcconfig
```