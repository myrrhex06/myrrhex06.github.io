---
title: "자료구조 - 정적 배열과 동적 배열이란?"
date: 2026-02-04 19:30:00 +0900
categories: [CS, DataStructure]
tags: [swift, java, array, index]
---

> 본 글은 [『얄코의 가장 쉬운 자료구조와 알고리즘』](https://www.inflearn.com/course/%EC%96%84%EC%BD%94%EC%9D%98-%EA%B0%80%EC%9E%A5-%EC%89%AC%EC%9A%B4-%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0%EC%99%80-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98?cid=337721)을 참고하여 개인 학습 목적으로 이해한 내용을 정리한 것입니다.

- 정적 배열: 배열의 크기가 고정되어 있는 배열
- 동적 배열: 배열의 크기 만큼 요소가 모두 찬 상태에서 새로운 요소를 추가하려고 할 경우 더 넓은 메모리 공간을 동적으로 할당해주는 배열
  - 단, 동적 배열도 내부적으로는 정적 배열을 사용하며, 공간이 부족해질 경우 더 큰 정적 배열을 새로 할당한 뒤 기존 요소들을 복사하는 방식으로 동작함.

> Swift에서 제공하는 `Array`는 동적 배열임.
{: .prompt-tip }

Java의 정적 배열을 활용한 동적 배열 동작 구현 예시
```java
public class Main {
    public static void main(String[] args) {
        String[] array = {"apple", "banana", "orange", "fruit"};
        String[] newArray = new String[array.length + 1];

        int insertIndex = 2;

        for(int i = 0; i < insertIndex; i++){
            newArray[i] = array[i];
        }

        newArray[insertIndex] = "hello";

        for(int i = insertIndex; i < newArray.length - 1; i++){
            newArray[i + 1] = array[i];
        }

        System.out.println(Arrays.toString(newArray));
    }
}
```

Java의 경우 위와 같이 직접 정적 배열을 활용하여 동적 배열을 구현해도 되긴하지만, `ArrayList`라는 클래스를 제공하기 때문에 해당 클래스를 통해 편리하게 동적 배열을 사용 가능함.