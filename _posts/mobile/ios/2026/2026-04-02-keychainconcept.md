---
title: "iOS - KeyChain Service API란?"
date: 2026-04-02 20:23:00 +0900
categories: [Mobile, iOS]
tags: [ios, keychain, account, encrypt, database]
---

## **KeyChain Service API 개념**

KeyChain Service API는 Apple이 제공하는 보안 프레임워크로, 사용자의 데이터를 비트 단위로 KeyChain이라 불리는 암호화 데이터베이스에 저장하도록 돕는 API임.

![image](/assets/img/keychainserviceapistructure.png)

> 출처: [Apple Developer Document - Keychain services](https://developer.apple.com/documentation/security/keychain-services)

위 이미지처럼 단순히 사용자의 비밀번호 뿐만 아니라 신용카드 정보나 짧은 노트 내용도 저장이 가능함.

KeyChain에 데이터를 저장하기 전에는 KeyChain Item으로 패키징을 해줘야함.

![image](/assets/img/keychainitem.png)

> 출처: [Apple Developer Document - Keychain items](https://developer.apple.com/documentation/security/keychain-items)

암호화하려는 데이터 뿐만 아니라 암호화된 데이터를 검색할 수 있도록 Attributes를 붙여서 패키징한 것이 KeyChain Item임.

KeyChain Service API를 통해 암호화된 데이터에 쉽게 접근이 가능하기 때문에 아래 이미지처럼 흐름을 구성하여 사용자 경험(UX)을 저해시키지 않고 인증 흐름을 구성할 수 있음.

![image](/assets/img/keychainexampleprocess.png)

> 출처: [Apple Developer Document - Using the keychain to manage user secrets](https://developer.apple.com/documentation/security/using-the-keychain-to-manage-user-secrets)

**흐름 설명**
- KeyChain에 저장된 인증 정보가 없다면?
  - 사용자에게 인증 정보를 입력받고 인증 성공 시 필요한 정보를 KeyChain에 저장
- KeyChain에 저장된 인증 정보가 있으나 모종의 이유(ex. 토큰 만료)로 인증이 실패한다면?
  - 사용자에게 인증 정보를 다시 입력받고 인증 성공 시 KeyChain에 저장된 데이터 업데이트
- KeyChain에 저장된 인증 정보가 있고 유효하다면 통과

## **KeyChain Service API 사용**

KeyChain 데이터 저장 예시

```swift
import Foundation
import Security

final class KeyChainUtil{
    
  private init() {}
    
  public static func create(key: String, data: String){
        
    // Query 생성
    let query: NSDictionary = [
      kSecClass: kSecClassGenericPassword,
      kSecAttrAccount: key,
      kSecAttrService: Constants.SERVICE_NAME,
      kSecValueData: data.data(using: .utf8, allowLossyConversion: false) as Any
    ]

    // 데이터 저장
    SecItemAdd(query, nil)
  }
}
```

- `kSecClass`: 저장되는 데이터 유형
- `kSecAttrAccount`: 데이터를 구분하기 위한 키값
- `kSecAttrService`: 데이터를 묶는 그룹 (네임스페이스 개념)

KeyChain 데이터 조회 예시

```swift
import Foundation
import Security

final class KeyChainUtil{

  public static func read(key: String) -> String?{

    // Query 생성
    let query: NSDictionary = [
        kSecClass: kSecClassGenericPassword,
        kSecAttrAccount: key,
        kSecAttrService: Constants.SERVICE_NAME,
        kSecReturnData: true
    ]
        
    var item: AnyObject?
        
    // 저장된 데이터 복사
    let status = SecItemCopyMatching(query, &item)
        
    if status == errSecSuccess{
      // 성공 시 데이터 복원
      return String(data: item as! Data, encoding: .utf8)
    }else{
      return nil
    }
  }
}
```

KeyChain 데이터 삭제

```swift
import Foundation
import Security

final class KeyChainUtil{

  public static func delete(key: String){

      // Query 생성
      let query: NSDictionary = [
          kSecClass: kSecClassGenericPassword,
          kSecAttrAccount: key,
          kSecAttrService: Constants.SERVICE_NAME
      ]

      // 데이터 삭제  
      SecItemDelete(query)
  }
}
```

## **Reference**
- [https://developer.apple.com/documentation/security/keychain-services](https://developer.apple.com/documentation/security/keychain-services)
- [https://developer.apple.com/documentation/security/keychain-items](https://developer.apple.com/documentation/security/keychain-items)
- [https://developer.apple.com/documentation/security/adding-a-password-to-the-keychain](https://developer.apple.com/documentation/security/adding-a-password-to-the-keychain)
- [https://developer.apple.com/documentation/security/using-the-keychain-to-manage-user-secrets](https://developer.apple.com/documentation/security/using-the-keychain-to-manage-user-secrets)