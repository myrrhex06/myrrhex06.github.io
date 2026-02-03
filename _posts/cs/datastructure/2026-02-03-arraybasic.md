---
title: "자료구조 - 배열(Array)이란?"
date: 2026-02-03 21:17:00 +0900
categories: [CS, DataStructure]
tags: [swift, array, index]
---

> 본 글은 [『얄코의 가장 쉬운 자료구조와 알고리즘』](https://www.inflearn.com/course/%EC%96%84%EC%BD%94%EC%9D%98-%EA%B0%80%EC%9E%A5-%EC%89%AC%EC%9A%B4-%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0%EC%99%80-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98?cid=337721)을 참고하여 개인 학습 목적으로 이해한 내용을 정리한 것입니다.

배열은 메모리상에서 모든 요소가 연속적으로 배치되어 있는 자료구조를 의미함.

배열에 저장되는 요소에는 요소의 위치를 특정할 수 있는 **인덱스** 값이 부여됨.

> 인덱스 값은 0부터 차례대로 부여됨.
{: .prompt-tip }

### **배열의 장점**
- 메모리 효율성이 높음.
- 인덱스를 통한 요소 접근이 빠름.

### **처리 성능**
- 배열 생성: 배열 크기에 비례 → O(n)
- 배열 요소 접근: 인덱스를 통해 순회 없이 바로 접근 가능 → O(1)
- 배열 요소 수정: 인덱스를 통해 순회 없이 바로 수정 가능 → O(1)
- 특정 값 탐색: 배열 요소를 모두 순회해서 찾아야함 → O(n)

예시
```swift
// 배열 선언
var arr: [String] = ["banana", "apple", "orange"]

// 배열 요소 조회
print(arr[1])

// 배열 요소 수정
arr[0] = "BANANA"

// 배열 요소 출력
print(arr)

// 특정 값 탐색
for index in 0..<arr.count{
    if arr[index] == "orange"{
        print("index : \(index), value : \(arr[index])")
    }
}
```