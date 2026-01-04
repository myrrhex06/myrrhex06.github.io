---
title: "CocoaPods로 SnapKit 의존성 추가하기"
date: 2025-12-22 21:52:00 +0900
categories: [Mobile, iOS]
tags: [xcode, cocoapods, dependency, gem, snapkit]
---

프로젝트 루트 디렉토리로 이동하여 아래 커맨드 실행

```bash
pod init
```

생성된 `Podfile`에 아래와 같이 `SnapKit` 의존성 추가

`Podfile`
```bash
# Uncomment the next line to define a global platform for your project
platform :ios, '11.0'

target 'YourProjectName' do
  # Comment the next line if you don't want to use dynamic frameworks
  use_frameworks!

  # Pods for YourProjectName
  pod 'SnapKit', '~> 5.0.0'

end
```

아래 커맨드 실행

```bash
pod install
```

> CocoaPods 사용 시 `.xcworkspace` 파일로 프로젝트를 열어야 함.
{: .prompt-tip }

## **Reference**
- [https://0urtrees.tistory.com/273](https://0urtrees.tistory.com/273)