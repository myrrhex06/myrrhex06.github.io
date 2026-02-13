---
title: "Troubleshooting - SnapKit.framework/Info.plist: utimensat (2): Operation not permitted 해결"
date: 2025-12-23 17:42:00 +0900
categories: [Troubleshooting]
tags: [xcode, cocoapods, gem]
---

## **이슈**

CocoaPods을 이용하여 SnapKit을 설치한 후 프로젝트를 빌드하니 아래와 같은 오류가 발생함.

![image](/assets/img/cocoapodssnapkitbuildissueimage.png)

## **해결**

Project Build Setting > Build Options > User Script Sandboxing 옵션 NO 설정

![image](/assets/img/cocoapodssnapkitbuildissuesolve.png)

## **Reference**
- [https://stackoverflow.com/questions/76590131/error-while-build-ios-app-in-xcode-sandbox-rsync-samba-13105-deny1-file-w](https://stackoverflow.com/questions/76590131/error-while-build-ios-app-in-xcode-sandbox-rsync-samba-13105-deny1-file-w)