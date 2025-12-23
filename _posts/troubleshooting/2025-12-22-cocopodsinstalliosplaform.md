---
title: "CocoaPods pod install Automatically assigning platform iOS with version 16.6 on target ... because no platform was specified. 해결"
date: 2025-12-22 21:40:00 +0900
categories: [Troubleshooting, iOS]
tags: [xcode, cocoapods, gem]
---

## **이슈**

CocoaPods을 이용하여 SnapKit을 설치하던 도중 아래와 같은 문제가 발생함.

![image](/assets/img/cocoapodsinstallwarntext.png)

## **해결**

`Podfile`에서 `platform` 주석을 해제한 후 버전을 11로 올려줌.

```
# Uncomment the next line to define a global platform for your project
platform :ios, '11.0'

...
```

다운로드를 다시하여 의존성을 추가해줌.

![image](/assets/img/cocoapodssnapkitdownloadsuccess.png)