---
title: "UIKit - TabBarController 설정 방법(스토리보드)"
date: 2025-10-29 06:52:00 +0900
categories: [Mobile, iOS]
tags: [uikit, storyboard, tabbarcontroller]
---

## **TabBarController 설정 방법(스토리보드)**

**1.`Storyboard`에서 `ViewController` 선택한 후 하위 메뉴에서 `Tab Bar Controller`를 선택**

![image](/assets/img/tabcontrollercreate.png)

생성된 `Tab Bar Controller` 확인

![image](/assets/img/tabcontrollercreatecheck.png)

**2.새로운 `ViewController`를 추가한 후 생성된 `Tab Bar Controller`를 선택한 후 `Control` 키를 누른 상태에서 드래그하여 새로 생성한 `ViewController`에 접근 (`Relationship Segue` 선택)**

![image](/assets/img/tabcontrollerconnect.png)

연결 확인

![image](/assets/img/tabcontrollerconnectcheck.png)

**3.ViewController에 생성된 Tab Item 요소를 클릭하여 설정을 통해 이미지와 타이틀 설정**

![image](/assets/img/tabitemsetting.png)

설정 적용 확인

![image](/assets/img/tabitemsettingcheck.png)

화면 구분을 위한 색상 지정

![image](/assets/img/finaltabcontrollersetting.png)

**결과**

![image](/assets/img/tabcontrolreslut.gif)