---
title: "UIKit - 이메일 주소 유효성 검증 처리하기"
date: 2026-01-02 06:40:00 +0900
categories: [Mobile, iOS]
tags: [ios, uikit, uitextfield, validate, regex, nspredicate]
---

이메일 주소의 유효성 검증은 정규 표현식과 `NSPredicate`를 통해 처리할 수 있음.

예시
```swift
import Foundation

final class ValidationUtil{

  ...

    /// 이메일 주소 유효성 검증 처리 메서드
    ///
    /// 매개변수로 받은 이메일 주소 값의 유효성을 검증
    ///
    /// - Parameter email: 이메일 주소
    /// - Throws: `ValidationError.requiredElement` 혹은 `ValidationError.invalidEmailFormat` 에러가 발생할 수 있음
    public static func validateEmail(email: String?) throws{
        guard let email = email, !email.isEmpty else {
            throw ValidationError.requiredElement
        }
        
        // 정규 표현식 변수에 할당
        let regex = "[A-Z0-9a-z._%+-]+@[A-Za-z0-9.-]+\\.[A-Za-z]{2,}"

        // NSPredicate 인스턴스 생성
        let predicate = NSPredicate(format: "SELF MATCHES %@", regex)
        
        // 유효성 검증
        let result = predicate.evaluate(with: email)

        if !result{
            throw ValidationError.invalidEmailFormat
        }
    }

  ...

}
```

## **Reference**
- [https://qkrrmsdud.tistory.com/57](https://qkrrmsdud.tistory.com/57)