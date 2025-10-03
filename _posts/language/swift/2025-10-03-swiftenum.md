---
title: "Swift - 열거형이란?"
date: 2025-10-03 19:56:00 +0900
categories: [Language, Swift]
tags: [swift, enum]
---

## **열거형이란?**
- 연관된 값들을 하나의 타입으로 묶어서 표현할 수 있음.
- 클래스와 구조체처럼 직접 초기화(`init`)할 수는 없음.

구문
```swift
enum 열거형명{
  case 멤버1
  case 멤버2
  ...
}
```

예시
```swift
enum Category{
    case exercise
    case music
    case friend
    case qna
}

var category: Category = Category.exercise
category = .music
print(category)
```

결과
```
music
```

> 위 예시처럼 열거형에 정의된 값을 사용할 수 있으며, 타입 어노테이션을 통해 열거형 타입임을 명시하면 열거형명을 생략하고 `.` 을 통해 바로 열거형 값에 접근 가능함.
{: .prompt-tip }

열거형은 `switch` 분기문과 함께 사용했을 때 시너지가 좋음.

예시
```swift
enum Category{
    case exercise
    case music
    case friend
    case qna
}

var category: Category = Category.exercise

switch category{
case .exercise:
    print("운동")
case .music:
    print("음악")
case .friend:
    print("친구")
case .qna:
    print("QnA")
}
```

결과
```
운동
```

> 열거형에 정의되어 있는 모든 값들을 `case` 문으로 사용할 경우 `default` 구문을 생략 가능함
{: .prompt-tip }

### **멤버에 값 할당**
- 열거형에 정의된 멤버에 값을 할당할 수 있음.
- `enum` 정의 구문에 할당할 값의 데이터 타입을 명시해줘야함.
- 할당된 값에는 `rawValue`라는 이름으로 접근 가능함.

예시
```swift
enum Category: String{
    case exercise = "운동"
    case music = "음악"
    case friend = "친구"
    case qna = "QnA"
}

print(Category.exercise.rawValue)
print(Category.music.rawValue)
print(Category.friend.rawValue)
print(Category.qna.rawValue)
```

결과
```
운동
음악
친구
QnA
```

또한, `Int` 형으로 값을 지정할 경우 자동 값 할당 기능을 사용할 수도 있음.

예시
```swift
enum Category: Int{
    case exercise = 1
    case music
    case friend
    case qna
}

print(Category.exercise.rawValue)
print(Category.music.rawValue)
print(Category.friend.rawValue)
print(Category.qna.rawValue)
```

결과
```
1
2
3
4
```

가장 처음 정의된 `exercise`가 1을 할당 받고 그 아래 정의된 값들은 차례대로 +1된 값들을 할당받음.

### **연관값 설정**
- 열거형은 멤버에 값을 직접 할당하는 것 외에도 연관값을 설정할 수 있음.

예시 1
```swift
enum NetworkResponse{
    case success(data: String)
    case fail(code: Int, message: String)
}


var response: NetworkResponse = NetworkResponse.success(data: "요청 성공")

switch response{
case .success(let data):
    print("요청 결과 \(data)")
case .fail(let code, let message):
    print("요청 결과 \(code), \(message)")
}
```

결과
```
요청 결과 요청 성공
```

예시 2
```swift
enum Category{
    case exercise(String)
    case music(Int, String)
    case movie(String)
}

var category: Category = .music(10, "노래이름")

switch category{
case .exercise(let value):
    print("값 \(value)")
case .music(let intValue, let stringValue):
    print("정수값 : \(intValue), 문자열값 : \(stringValue)")
default:
    print("default")
}
```

결과
```
정수값 : 10, 문자열값 : 노래이름
```

### **메서드, 프로퍼티 정의**
- 열거형은 연산 프로퍼티, 타입 프로퍼티, 메서드를 가질 수 있음.

> 저장 프로퍼티는 갖을 수 없음.
{: .prompt-tip }

예시
```swift
enum Category{
    case exercise
    case music
    case qna
    
    static var typeProperty: String = "Hello World"
    
    var description: String{
        get{
            switch self{
            case .exercise:
                return "운동"
            case .music:
                return "음악"
            case .qna:
                return "질문"
            }
        }
    }
    
    func getCategoryIndex() -> Int{
        switch self{
        case .exercise:
            return 0
        case .music:
            return 1
        case .qna:
            return 2
        }
    }
}

print(Category.typeProperty)

var category: Category = Category.exercise
print(category.description)
print(category.getCategoryIndex())
```

결과
```
Hello World
운동
0
```