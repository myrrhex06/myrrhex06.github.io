---
title: "오토 레이아웃 동적 변경 애니메이션 구현 방법"
date: 2025-10-23 20:24:00 +0900
categories: [Mobile, iOS]
tags: [uikit, code, autolayout, animate, constraint, uiview]
---

## **오토 레이아웃 동적 변경 애니메이션 구현 방법**
오토 레이아웃을 동적으로 설정하여 `view`가 움직일 때 애니메이션을 입히는 방법은 `UIView`의 `animate` 메서드를 통해 처리 가능함.

예시
```swift
// 동적 오토레이아웃 설정에 애니메이션 효과를 넣어줌
UIView.animate(withDuration: 0.3) {

  ...

  // layoutIfNeeded: 변경된 제약조건을 즉시 반영하도록 강제로 Layout을 업데이트함.
  // 또한, 하위 View에 모두 적용됨.
  self.viewName.layoutIfNeeded()
}
```