---
title: "DataStructure - 단일 연결 리스트(Singly Linked List)란?"
date: 2026-02-05 21:07:00 +0900
categories: [CS, DataStructure]
tags: [swift, node, linkedlist]
---

> 본 글은 [『얄코의 가장 쉬운 자료구조와 알고리즘』](https://www.inflearn.com/course/%EC%96%84%EC%BD%94%EC%9D%98-%EA%B0%80%EC%9E%A5-%EC%89%AC%EC%9A%B4-%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0%EC%99%80-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98?cid=337721)을 참고하여 개인 학습 목적으로 이해한 내용을 정리한 것입니다.

한 요소가 다른 요소를 가리키는 형태로, 각 요소들은 각자의 데이터와 다음 요소의 참조를 가지고 있음.

메모리상에서는 배열과 달리 연속되지 않은 공간에 위치하여 참조를 통해 서로를 가리키는 형태임.

### **처리 성능**
- 요소 접근: 요소가 있는 위치까지 참조를 통해 요소들을 타고 들어가야함 → O(n)
- 요소 삽입: 배열과 달리 기존 요소들을 뒤로 밀어주는 것을 하지 않고 앞 뒤 요소의 참조만 변경해주면 됨.
    - 탐색이 필요할 경우 O(n), 위치를 알고 있을 경우 O(1)
- 연결 리스트 요소 삭제: 배열과 달리 뒤에 있는 요소들을 앞으로 당겨주지 않고 앞 뒤 요소의 참조만 변경해주면 됨
    - 탐색이 필요한 경우 O(n), 위치를 알고 있을 경우 O(1)

구현 예제
```swift
class Node{
    var data: Int
    var next: Node?
    
    init(data: Int) {
        self.data = data
        self.next = nil
    }
}

class LinkedList{
    var head: Node?
    
    func insert(data: Int){
        var newNode: Node = Node(data: data)
        newNode.next = head
        head = newNode
    }
    
    func delete(key: Int){
        var current = head
        var prev: Node? = nil
        
        while current != nil{
            if current?.data == key{
                if prev != nil{
                    prev?.next = current?.next
                }else{
                    head = current?.next
                }

                break
            }
            
            prev = current
            current = current?.next
        }
    }
    
    func search(key: Int) -> Bool{
        var current = head
        while current != nil{
            if current?.data == key { return true }
            current = current?.next
        }
        
        return false
    }
    
    func description(){
        var current = head
        while current != nil{
            print("\(current!.data) -> ", terminator: "")
            current = current?.next
        }
        print("nil")
    }
}

var list = LinkedList()
list.insert(data: 10)
list.insert(data: 20)
list.insert(data: 30)
list.insert(data: 40)
list.insert(data: 50)

list.delete(key: 30)
print("search : \(list.search(key: 20))")
list.description()
```

출력 결과
```
search : true
50 -> 40 -> 20 -> 10 -> nil
```