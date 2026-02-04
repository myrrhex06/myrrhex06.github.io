---
title: "자료구조 - 배열 요소의 삭입 & 삭제"
date: 2026-02-04 19:08:00 +0900
categories: [CS, DataStructure]
tags: [swift, java, array, index]
---

> 본 글은 [『얄코의 가장 쉬운 자료구조와 알고리즘』](https://www.inflearn.com/course/%EC%96%84%EC%BD%94%EC%9D%98-%EA%B0%80%EC%9E%A5-%EC%89%AC%EC%9A%B4-%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0%EC%99%80-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98?cid=337721)을 참고하여 개인 학습 목적으로 이해한 내용을 정리한 것입니다.

- 배열 요소 삭제: 맨 앞 혹은 가운데에 위치한 배열 요소를 삭제할 경우 삭제된 요소 뒤에 위치해 있던 요소들을 앞으로 한칸 씩 당겨줘야함 → O(n)
- 배열 요소 삽입: 맨 앞 혹은 가운데에 새로운 배열 요소를 삽입할 경우 기존에 있던 요소들을 뒤로 밀어줘야함 → O(n)

Java의 정적 배열을 통해 구현하게 되면 아래와 같이 요소 삭제 시에는 삭제된 요소 뒤에 위치한 요소들을 직접 앞으로 한 칸씩 당겨줘야하며, 요소 삽입 시에는 기존에 있던 요소들을 직접 뒤로 밀어줘야함.

예시
```java
import java.util.Arrays;

public class Main {
    public static void main(String[] args) {
        String[] arr = {"apple", "banana", "orange", "fruit"};

        // 배열 요소 삭제
        
        // 배열 요소를 당겨줌
        for(int i = 1; i < arr.length - 1; i++){
            arr[i] = arr[i + 1];
        }

				// 빈 공간 할당
        arr[arr.length - 1] = null;

        System.out.println(Arrays.toString(arr));

        // 배열 요소 추가

				// 배열 요소를 밀어줌
        for(int i = arr.length - 1; i > 0; i--){
            arr[i] = arr[i - 1];
        }

				// 배열 요소 삽입
        arr[0] = "hello";

        System.out.println(Arrays.toString(arr));
    }
}
```

Swift의 경우 기본적으로 동적 배열을 지원하기 때문에 요소 삭제, 삽입 시에 직접 요소들의 위치를 조정해주지 않아도 됨.

> 동적 배열을 사용한다고 해서 성능이 더 빨라지지 않으며, 정적 배열과 동일하게 O(n)임.
{: .prompt-tip }

예시
```swift
// 배열 선언
var arr: [String] = ["banana", "apple", "orange"]

// 배열 요소 삭제 ( Swift는 동적 배열이기 때문에 요소를 당겨주지 않아도 됨 )
arr.remove(at: 0)
print(arr)

// 배열 요소 삽입 ( Swift는 동적 배열이기 때문에 요소를 밀어주지 않아도 됨 )
arr.insert("hello", at: 0)
print(arr)
```