---
title: "UIKit - NavigationController 설정 방법(스토리보드)"
date: 2025-10-28 21:25:00 +0900
categories: [Mobile, iOS]
tags: [uikit, storyboard, navigationcontroller]
---

## **NavigationController (스토리보드)**
1.`Storyboard`에서 기본 제공되는 `ViewController` 외에 새로운 `ViewController` 생성

![image](/assets/img/basicstoryboard.png)

2.하단 메뉴를 통해 `NavigationController` 설정

![image](/assets/img/navigationcontrollersetting.png)

설정된 `NavigationController` 확인 

![image](/assets/img/navigationcontrollercreate.png)

3.`Segue` 설정 (`show` 선택)

![image](/assets/img/navigationshowconnect.png)

> `present modally`는 아래에서 위로 올라오지만, `show` 방식은 옆으로 넘어가는 효과임.
{: .prompt-tip }

4.`Segue` 설정 확인

![image](/assets/img/navigationshowconnectcheck.png)

5.`Navigation title` 설정

![image](/assets/img/navigationtitleprepare.png)

6.`Navigation title` 설정 확인

![image](/assets/img/navigationtitlesettingcheck.png)

결과

![image](/assets/img/navigationresult.gif)