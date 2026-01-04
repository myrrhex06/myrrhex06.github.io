---
title: "UIKit - UIDevice를 통해 기기 정보 확인하기"
date: 2025-12-03 18:02:00 +0900
categories: [Mobile, iOS]
tags: [uikit, code, uidevice]
---

`UIDevice`의 `current` 프로퍼티를 통해 현재 사용자의 기기 정보를 조회할 수 있음.

**사용 예시**

`UIDeviceUtil.swift`
```swift
import UIKit

class UIDeviceUtil{
    
    private init() {}
    
    // 사용자의 디바이스 이름 조회
    public static func getName() -> String{
        
        return UIDevice.current.name
    }
    
    // 사용자의 디바이스 UUID 조회
    public static func getUuid() -> String{
        
        return UIDevice.current.identifierForVendor?.uuidString ?? ""
    }
}
```

`TodoAddView.swift`
```swift
import UIKit

class TodoAddView: UIView {

    ...

    override init(frame: CGRect) {
        super.init(frame: frame)
        
        let name = UIDeviceUtil.getName()

        let uuid = UIDeviceUtil.getUuid()
        
        print("name : \(name), uuid = \(uuid)")
        
        ...
    }

    ....
}
```

결과

```
name : iPhone SE (3rd generation), uuid = 97D888F0-C6AA-43A9-AF72-B49CB59B129D
```

## **Reference**
- [https://youbidan-project.tistory.com/312](https://youbidan-project.tistory.com/312)