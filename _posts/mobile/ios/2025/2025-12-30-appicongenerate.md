---
title: "iOS App 출시를 위한 아이콘 제작하기"
date: 2025-12-30 21:24:00 +0900
categories: [Mobile, iOS]
tags: [ios, icon, xcode, appstore]
---

아이콘 제작을 위해 [https://www.appicon.co/](https://www.appicon.co/) 사이트에 접속

표시한 부분에 제작한 이미지 드래그 후 Generate 클릭

![image](/assets/img/iosicongenerator.png)

다운로드 받은 디렉토리 압축 해제한 뒤 Assets.xcassets > AppIcon.appiconset 디렉토리로 접근하여 1024.png 파일만 복사함.

iOS 앱 프로젝트 -> Assets -> AppIcon에 복사해둔 1024.png 파일을 붙여넣은 후 빌드하면 됨.

![image](/assets/img/xcodeiconapply.png)

## **Reference**
- [https://gran007.tistory.com/entry/ios-android-%EC%95%B1-%EC%95%84%EC%9D%B4%EC%BD%98-%EB%A7%8C%EB%93%A4%EA%B8%B0#google_vignette](https://gran007.tistory.com/entry/ios-android-%EC%95%B1-%EC%95%84%EC%9D%B4%EC%BD%98-%EB%A7%8C%EB%93%A4%EA%B8%B0#google_vignette)