---
title: "Swift - 딕셔너리(Dictionary)란?"
date: 2025-09-21 11:43:00 +0900
categories: [Language, Swift]
tags: [swift, dictionary, collection, tuple]
---

## **딕셔너리(Dictionary)란?**
- 데이터를 `key-value` 형식으로 저장하는 집단 자료형.
- 중복된 `key` 값은 저장할 수 없으며, 같은 `key` 값을 통해 데이터를 저장하려 할 경우 기존에 저장되있던 데이터가 수정됨.

### **선언 및 초기화**
형식
```swift
// 1.
var d1: Dictionary<key_type, value_type> = Dictionary()

// 2.
var d1: [key_type:value_type] = [key_type:value_type]()
```

예시
```swift
var d1: Dictionary<String, Int> = Dictionary()
var d2: [String : Int] = [String : Int]()
```

### **데이터 삽입**
데이터 삽입에는 아래 두 가지 방법 중 하나를 사용 가능함.
- 직접 `key-value` 값 할당.
- `updateValue(_, forKey)` 메서드 사용.
  - 해당 메서드의 `forKey` 값으로 넘어간 `key` 값이 이미 딕셔너리에 저장되어 있을 경우 데이터 수정이 일어남.
 
예시
```swift
var d1: Dictionary<String, Int> = Dictionary()
var d2: [String : Int] = [String : Int]()

// 직접 값 할당
d1["KR"] = 1
d1["EN"] = 2

// updateValue 메서드 사용
d2.updateValue(1, forKey: "KR")
d2.updateValue(2, forKey: "EN")
```

> `updateValue` 메서드 사용 시 데이터 수정이 일어날 경우 기존에 저장되어 있던 데이터가 `Optional` 타입으로 반환됨.
{: .prompt-tip }

### **데이터 접근**
`key` 값을 통해 딕셔너리에 접근 가능함.

예시
```swift
var d1: Dictionary<String, Int> = Dictionary()
d1["KR"] = 1

print(d1["KR"])
```

결과
```
Optional(1)
```

위 예시에서 확인 가능하듯이 딕셔너리에 `key` 값을 통해 접근할 경우 반환값은 `Optional` 타입으로 반환됨.

### **데이터 삭제**
딕셔너리에 저장된 데이터를 삭제하는 방법은 아래 두 방법 중 하나를 사용 가능함.
- 직접 `nil` 할당
- `removeValue(forKey)` 메서드 사용

예시
```swift
var d1: Dictionary<String, Int> = Dictionary()
d1["KR"] = 1
d1["EN"] = 2

d1["KR"] = nil
d1.removeValue(forKey: "EN")
```

> `removeValue` 메서드를 통해 데이터를 삭제할 경우, 반환 값으로 삭제된 데이터를 `Optional` 타입으로 반환해줌.
{: .prompt-tip }

### **데이터 순회 탐색**
- 딕셔너리을 순회 탐색할 경우 값을 튜플 타입처럼 다룰 수 있게 때문에, 변수 바인딩을 통해 편리하게 처리가 가능함.

예시
```swift
var d1: Dictionary<String, Int> = Dictionary()
d1["KR"] = 1
d1["EN"] = 2

for row in d1{
    let (key, value) = row
    print("\(key) : \(value)")
}
```

결과
```
EN : 2
KR : 1
```

> 딕셔너리는 순서를 보장해주지 않기 때문에, 출력 결과의 순서는 제각각임.
{: .prompt-tip }