---
title: "UIKit - UIButton 디자인 유지하면서 disable 처리하기"
date: 2025-11-29 20:58:00 +0900
categories: [Mobile, iOS]
tags: [uikit, code, uibutton]
---

`UIButton`의 `isEnable` 프로퍼티를 `false`로 설정하여 `disable` 처리를 하면 아래와 같이 기존 `UIButton` 디자인이 흐려짐.

![image](/assets/img/useisenablefalse.gif)

구성해둔 디자인은 유지한 채로 `disable` 처리를 하는 것은 `isUserInteractionEnabled` 프로퍼티를 사용하면 됨.

```swift
btn.isUserInteractionEnabled = false
```

> `isEnabled = false`는 시스템이 버튼의 비활성화 스타일을 적용하여(UI 흐려짐) 명확히 비활성 상태임을 보여준다면,  `isUserInteractionEnabled = false`는 UI 스타일 변화 없이 터치 이벤트만 차단함.
{: .prompt-tip }

결과

![image](/assets/img/useisuserinteractionenabled.gif)

## Reference
- [https://stackoverflow.com/questions/6210850/how-do-i-disable-a-uibutton](https://stackoverflow.com/questions/6210850/how-do-i-disable-a-uibutton)