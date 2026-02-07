---
title: "자료구조 - 이중 연결 리스트란?"
date: 2026-02-07 16:16:00 +0900
categories: [CS, DataStructure]
tags: [swift, node, doublelinkedlist]
---

> 본 글은 [『얄코의 가장 쉬운 자료구조와 알고리즘』](https://www.inflearn.com/course/%EC%96%84%EC%BD%94%EC%9D%98-%EA%B0%80%EC%9E%A5-%EC%89%AC%EC%9A%B4-%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0%EC%99%80-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98?cid=337721)을 참고하여 개인 학습 목적으로 이해한 내용을 정리한 것입니다.

이중 연결 리스트는 각 요소가 이전 요소와 다음 요소의 참조값을 가지는 형태임.

따라서 정방향 뿐만 아니라 역방향 순회가 가능함.

### **처리 성능**
- 요소 삽입: 맨 처음 요소(head), 맨 뒤에 요소(tail)에 삽입할 경우 O(1), 그렇지 않으면 순회가 필요하기 때문에 O(n)
- 요소 제거: 맨 처음 요소(head), 맨 뒤에 요소(tail)를 삭제할 경우 O(1), 그렇지 않으면 순회가 필요하기 때문에 O(n)
- 요소 탐색: 다음 요소의 참조를 타고 계속 들어가야 하기 때문에 O(n)
  - 요소 탐색 시작 지점을 맨 처음 요소(head), 맨 뒤 요소(tail) 중 선택 가능

구현 예시
```swift
// MARK: - Node
class Node{
    var data: Int
    var next: Node?
    var prev: Node?
    
    init(data: Int) {
        self.data = data
        self.next = nil
        self.prev = nil
    }
}

// MARK: - LinkedList
class DoubleLinkedList{
    private var head: Node?
    private var tail: Node?
    
    public func insertAtHead(data: Int){
        let newNode = Node(data: data)
        if head == nil{
            head = newNode
            tail = newNode
        }else{
            newNode.next = head
            head?.prev = newNode
            head = newNode
        }
    }
    
    public func insertAtTail(data: Int){
        let newNode = Node(data: data)
        if tail == nil{
            head = newNode
            tail = newNode
        }else{
            newNode.prev = tail
            tail?.next = newNode
            tail = newNode
        }
    }
    
    public func deleteAtHead(){
        if head == nil { return }
        
        if head === tail{
            head = nil
            tail = nil
            return
        }
        
        head = head?.next
        head?.prev = nil
    }
    
    public func deleteAtTail(){
        if tail == nil { return }
        
        if head === tail{
            head = nil
            tail = nil
            return
        }
        
        tail = tail?.prev
        tail?.next = nil
    }
    
    public func searchFromHead(data: Int) -> Bool{
        var current = head
        while current != nil{
            if current?.data == data{
                return true
            }
            
            current = current?.next
        }
        
        return false
    }
    
    public func searchFromTail(data: Int) -> Bool{
        var current = tail
        while current != nil{
            if current?.data == data{
                return true
            }
            
            current = current?.prev
        }
        
        return false
    }
    
    public func traverse(){
        var current = head
        
        while current != nil{
            print("\(current!.data) -> ", terminator: "")
            current = current?.next
        }
        
        print("nil")
    }
}

// MARK: - Example
let list = DoubleLinkedList()
list.insertAtHead(data: 1)
list.insertAtHead(data: 2)
list.insertAtHead(data: 3)

list.traverse()

list.deleteAtHead()

list.traverse()
```

출력 결과
```
3 -> 2 -> 1 -> nil
2 -> 1 -> nil
```