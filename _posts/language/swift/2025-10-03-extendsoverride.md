---
title: "Swift - 상속과 오버라이딩이란?"
date: 2025-10-03 09:56:00 +0900
categories: [Language, Swift]
tags: [swift, class, property, method, override, final]
---

## **상속이란?**
- 한 클래스에서 정의한 메서드, 프로퍼티를 다른 클래스에서 그대로 물려받는 것을 의미함.
- 상속을 받은 하위 클래스는 상위 클래스에 정의된 프로퍼티, 메서드를 재정의하여 사용할 수 있음.
- 클래스에서만 지원하는 기능이라 구조체에서는 불가능함.

> Swift는 다중 상속을 지원하지 않기 때문에 하나의 클래스만 상속 가능함.
{: .prompt-tip }

구문
```swift
class 하위클래스: 상위클래스{
  ...
}
```

예시
```swift
class Animal{
    var gender: String = "수컷"
    var age: Int = 4
    
    func makeSound(){
        print("울음소리")
    }
}

class Dog: Animal{
    
}

var animal: Animal = Animal()
animal.makeSound()
print(animal.gender)
print(animal.age)

var dog: Dog = Dog()
dog.makeSound()
print(dog.gender)
print(dog.age)
```

결과
```
울음소리
수컷
4
울음소리
수컷
4
```

위 예시에서 상위 클래스에서 정의한 프로퍼티, 메서드를 하위 클래스에서 모두 사용할 수 있다는 것을 확인 가능함.

## **오버라이딩이란?**
- 상위 클래스에서 정의한 프로퍼티, 메서드를 재정의하는 것을 의미함.
- 오버라이딩 시에는 `override` 키워드를 통해 오버라이딩 했음을 컴파일러에게 알려줘야함.
- 오버라이딩은 하위 클래스들에게만 적용되고, 상위 클래스에는 아무런 영향이 없음.

### **프로퍼티 오버라이딩**
- 프로퍼티 오버라이딩 시에는 연산 프로퍼티(`get`, `set` 구문 모두 존재)로 오버라이딩 해줘야함.
  - 저장 프로퍼티를 저장 프로퍼티로, 연산 프로퍼티를 저장 프로퍼티로 오버라이딩할 수 없다는 의미임.
  - 상위 클래스에 정의된 연산 프로퍼티가 `get` 구문만 존재하는 읽기 전용 연산 프로퍼티라면, 예외적으로 오버라이딩 시에 `get` 구문만 재정의 가능함.
- 저장 프로퍼티를 오버라이딩할 경우 연산 프로퍼티로 재정의 하지 않고 프로퍼티 옵저버(`willSet`, `didSet`)로 hook 처리만 재정의하는 것도 가능함.

예시 1
```swift
class Animal{
    var gender: String = "수컷"
    var age: Int = 4
    
    func makeSound(){
        print("울음소리")
    }
}

class Dog: Animal{
    override var gender: String{
        get{
            return "암컷"
        }
        set{
            print("새로운 값 : \(newValue)")
        }
    }
    
    override var age: Int{
        get{
            return 3
        }
        set{
            print("새로운 값 : \(newValue)")
        }
    }
}

var animal: Animal = Animal()
print(animal.gender)
print(animal.age)

var dog: Dog = Dog()
print(dog.gender)
print(dog.age)
```

결과
```
수컷
4
암컷
3
```

예시 2
```swift
class Animal{
    var gender: String = "수컷"
    
    func makeSound(){
        print("울음소리")
    }
}

class Dog: Animal{
    override var gender: String{
        willSet{
            print("기존 값 : \(gender) 새로 들어온 값 : \(newValue)")
        }
        didSet{
            print("오래된 값 : \(oldValue) 새로 할당된 값 : \(gender)")
        }
    }
}

var animal: Animal = Animal()
print(animal.gender)

var dog: Dog = Dog()
dog.gender = "암컷"
print(dog.gender)
```

결과
```
수컷
기존 값 : 수컷 새로 들어온 값 : 암컷
오래된 값 : 수컷 새로 할당된 값 : 암컷
암컷
```

### **메서드 오버라이딩**
- 상위 클래스에서 정의된 메서드의 이름, 매개변수명, 매개변수 타입, 반환 타입이 모두 일치해야함.
  - 재정의할 수 있는건 실행 구문밖에 없단 소리임.

예시
```swift
class Animal{
    var gender: String = "수컷"
    
    func makeSound(){
        print("울음소리")
    }
    
    func getAge() -> Int{
        return 4
    }
}

class Dog: Animal{
    override func makeSound() {
        print("멍멍")
    }
    
    override func getAge() -> Int {
        return 3
    }
}

var animal: Animal = Animal()
animal.makeSound()
print(animal.getAge())

var dog: Dog = Dog()
dog.makeSound()
print(dog.getAge())
```

결과
```
울음소리
4
멍멍
3
```

### **오버라이딩 제한**
- `final` 키워드릴 통해서 클래스에 정의된 메서드, 프로퍼티를 오버라이딩하는 것을 막을 수 있음.
  - `final` 키워드를 클래스 선언 구문에 붙여 클래스를 상속하는 것도 막을 수 있음.

예시 1
```swift
class Animal{
    var gender: String = "수컷"
    
    final func makeSound(){
        print("울음소리")
    }
    
    final func getAge() -> Int{
        return 4
    }
}
```

예시 2
```swift
final class Animal{
    var gender: String = "수컷"
    
    ...
}
```