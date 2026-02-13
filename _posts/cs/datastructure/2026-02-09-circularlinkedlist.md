---
title: "DataStructure - 원형 연결 리스트(Circular Linked List)란?"
date: 2026-02-09 21:22:00 +0900
categories: [CS, DataStructure]
tags: [swift, node, circularlinkedlist]
---

> 본 글은 [『얄코의 가장 쉬운 자료구조와 알고리즘』](https://www.inflearn.com/course/%EC%96%84%EC%BD%94%EC%9D%98-%EA%B0%80%EC%9E%A5-%EC%89%AC%EC%9A%B4-%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0%EC%99%80-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98?cid=337721)을 참고하여 개인 학습 목적으로 이해한 내용을 정리한 것입니다.

원형 연결 리스트란 마지막 요소(tail)가 첫 번째 요소(head)를 가리켜서 원형같은 구조를 이루는 리스트임.

> 마지막 요소가 첫 번째 요소와 연결되어 있기 때문에 끊임없이 순회 가능함.
{: .prompt-tip }

### **처리 성능**
- 요소 삽입: head, tail에 삽입할 경우 O(1), 특정 위치에 삽입할 경우 순회가 필요하기 때문에 O(n)
- 요소 삭제: head, tail 요소를 삭제할 경우 O(1), 특정 위치에 요소를 삭제할 경우 순회가 필요하기 때문에 O(n)
- 요소 탐색: 요소의 참조를 통해 순회하기 때문에 O(n)

구현 예시
```swift
class Node{
    var data: Int
    var next: Node?
    var prev: Node?
    
    init(data: Int, next: Node? = nil, prev: Node? = nil) {
        self.data = data
        self.next = next
        self.prev = prev
    }
}

class LinkedList{
    var head: Node?
    var current: Node?
    
    public func insertAtHead(data: Int){
        var newNode = Node(data: data)
        if head == nil{
            head = newNode
            head?.next = head
            head?.prev = head
            current = head
        }else{
            var tail = head?.prev
            newNode.next = head
            newNode.prev = tail
            head?.prev = newNode
            tail?.next = newNode
            head = newNode
        }
    }
    
    public func insertAtTail(data: Int){
        var newNode = Node(data: data)
        if head == nil{
            head = newNode
            head?.next = newNode
            head?.prev = newNode
            current = newNode
        }else{
            var tail = head?.prev
            newNode.next = head
            newNode.prev = tail
            head?.prev = newNode
            tail?.next = newNode
        }
    }
    
    public func deleteAtHead(){
        if head == nil{ return }
        
        if head?.next === head{
            head = nil
            current = nil
            return
        }
        
        var tail = head?.prev
        if current === head { current = head?.next }
        head = head?.next
        head?.prev = tail
        tail?.next = head
    }
    
    public func deleteAtTail(){
        if head == nil { return }
        
        if head?.next === head{
            head = nil
            current = nil
            return
        }
        
        var tail = head?.prev
        if current === tail { current = head }
        var newTail = tail?.prev
        newTail?.next = head
        head?.prev = newTail
    }
    
    public func printList(){
        if head == nil { return }
        
        var temp = head
        repeat{
            print("\(temp!.data) -> ", terminator: "")
            temp = temp?.next
        }while temp !== head
        
        print("nil")
    }
    
    public func moveNext(){
        if current == nil { return }
        current = current?.next
    }
    
    public func movePrev(){
        if current == nil { return }
        current = current?.prev
    }
    
    public func printCurrent(){
        guard let current = current else {
            print("current : nil")
            return
        }
        
        print("current : \(current.data)")
    }
}

var list = LinkedList()
list.insertAtHead(data: 64)
list.insertAtHead(data: 33)
list.insertAtHead(data: 91)
list.insertAtHead(data: 99)
list.insertAtHead(data: 15)
list.insertAtHead(data: 53)

list.printList()

list.deleteAtTail()

list.printList()

list.current = list.head;
list.printCurrent();

for i in 0..<3{
    list.moveNext();
}

list.printCurrent();
```