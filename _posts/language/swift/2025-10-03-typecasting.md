---
title: "Swift - 타입 캐스팅이란?"
date: 2025-10-03 12:04:00 +0900
categories: [Language, Swift]
tags: [swift, class, type, as]
---

- 하위 클래스 인스턴스는 상위 클래스 타입으로 선언된 변수에 할당 가능함.
  - 하위 클래스는 상위 클래스에 선언된 모든 프로퍼티, 메서드를 가지고 있기 때문임.
  - 반대가 되는 경우는 성립하지 않는데, 하위 클래스에서 추가된 프로퍼티, 메서드를 상위 클래스에서는 알 수 없기 때문임.

예시
```swift
class Animal{
    var gender: String = "수컷"
    var age: Int = 3
    
    func makeSound(){
        print("동물소리")
    }
}

class Dog: Animal{
    var weight: Double = 5.7
    
    override func makeSound() {
        print("월월")
    }
}

var dog: Animal = Dog()
dog.makeSound()
```

결과
```
월월
```

> 하위 클래스에서 추가된 프로퍼티나 메서드에는 접근이 불가능하지만, 오버라이딩한 내용은 반영됨.
{: .prompt-tip }

## **타입 비교 연산**
- `is`를 통해 타입을 비교 연산할 수 있음.

구문
```swift
값 is 타입
```

예시
```swift
class Animal{
    var gender: String = "수컷"
    var age: Int = 3
    
    func makeSound(){
        print("동물소리")
    }
}

class Dog: Animal{
    var weight: Double = 5.7
    
    override func makeSound() {
        print("월월")
    }
}

print(Dog() is Animal)
print(Dog() is Dog)
```

결과
```
true
true
```

> 하위 타입을 상위 타입과 비교할 때에도 `true`가 반환됨.
{: .prompt-tip }

**주의점**

- 변수에 할당된 인스턴스를 비교할 때는 변수 데이터 타입이 아닌 실제로 할당되어 있는 인스턴스를 기준으로 비교 연산을 처리함.

에시
```swift
class Animal{
    var gender: String = "수컷"
    var age: Int = 3
    
    func makeSound(){
        print("동물소리")
    }
}

class Dog: Animal{
    var weight: Double = 5.7
    
    override func makeSound() {
        print("월월")
    }
}

var animal: Animal = Dog()

print(animal is Animal)
print(animal is Dog)
```

결과
```
true
true
```

위 예시에서는 변수 타입은 `Animal`이지만 실제 할당된 인스턴스는 `Dog`임.<br>
비교 연산 결과 `Animal`로 비교 연산이 처리된 것이 아닌 `Dog`로 처리된 것을 확인할 수 있음.

## **형변환**
- 특정 타입으로 선언된 값을 다른 타입으로 변환하는 것.
- 일반적으로 상속 관계에 있는 타입들에 한에서만 사용 가능함.

형변환은 두가지 유형이 있음

- **업캐스팅**: 특정 타입보다 상위 타입으로 변환
- **다운캐스팅**: 특정 타입보다 하위 타입으로 변한

### **업캐스팅**
- 특정 타입보다 상위 타입으로 변환되는 것이기 때문에 오류 가능성이 없음

구문
```swift
객체 as 타입
```

예시
```swift
class Animal{
    var gender: String = "수컷"
    var age: Int = 3
    
    func makeSound(){
        print("동물소리")
    }
}

class Dog: Animal{
    var weight: Double = 5.7
    
    override func makeSound() {
        print("월월")
    }
}

class Poodle: Dog{
    var cuteRate: Int = 100
}

var dog: Dog = Dog()
var animal = dog as Animal
print(animal.gender)
print(animal.age)
animal.makeSound()
```

결과
```
수컷
3
월월
```

### **다운 캐스팅**
- 특정 타입보다 하위 타입으로 변환되기 때문에 오류 가능성이 있음
- 다운 캐스팅은 옵셔널 캐스팅, 강제 캐스팅으로 나뉨.
    - 옵셔널 캐스팅은 오류가 발생할 수 있기 때문에 옵셔널로 감싸져서 나와짐
    - 강제 캐스팅은 무조건 성공한다는 전제가 깔려있기 때문에 실패시 런타임 에러가 발생함.

구문
```swift
객체 as? 타입 // 옵셔널 캐스팅
객체 as! 타입 // 강제 캐스팅
```

예시
```swift
class Animal{
    var gender: String = "수컷"
    var age: Int = 3
    
    func makeSound(){
        print("동물소리")
    }
}

class Dog: Animal{
    var weight: Double = 5.7
    
    override func makeSound() {
        print("월월")
    }
}

class Poodle: Dog{
    var cuteRate: Int = 100
}

var dog: Dog = Poodle()
var poodle = dog as? Poodle
if let poodle = poodle{
    print(poodle.cuteRate)
}else{
    print("형변환 실패")
}

var poodle2 = dog as! Poodle
print(poodle2.cuteRate)
```

결과
```
100
100
```