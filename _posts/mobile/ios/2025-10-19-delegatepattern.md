---
title: "델리게이트 패턴이란?"
date: 2025-10-19 17:10:00 +0900
categories: [Mobile, iOS]
tags: [uikit, storyboard, delegate, textfield]
---

## **델리게이트 패턴이란?**
UIKit에서 제공하는 거의 모든 UI에서 사용되기 때문에 핵심과도 같은 패턴임.

이 패턴은 이벤트를 자신이 직접 처리하는 것이 아닌 프로토콜을 통해 해당 프로토콜을 구현한 대리자에게 처리를 위임하는 패턴을 의미함.

델리게이트 패턴 구현 예시
```swift
import Foundation

// UITextFieldDelegate
@objc
protocol DriverbarDelegate{
    func isTurnLeft() -> Bool
    func isTurnRight() -> Bool
    
    @objc optional func turnLeft()
    @objc optional func turnRight()
}

// TextField
class Driverbar{
    var delegate: DriverbarDelegate?
    
    func turnLeft(){
      // 예시 코드이기 때문에 강제 언래핑을 통해 처리
        if (delegate?.isTurnLeft())!{
            delegate?.turnLeft?()
        }
    }
    
    func turnRight(){
      // 예시 코드이기 때문에 강제 언래핑을 통해 처리
        if (delegate?.isTurnRight())!{
            delegate?.turnRight?()
        }
    }
}

// ViewController
class ExCar: DriverbarDelegate{
    
    func isTurnLeft() -> Bool {
        print("ExCar isTurnLeft 호출")
        
        return true
    }
    
    func isTurnRight() -> Bool {
        print("ExCar isTurnRight 호출")
        
        return true
    }
    
    func turnLeft() {
        print("ExCar turnLeft 호출")
    }
    
    func turnRight() {
        print("ExCar turnRight 호출")
    }
}

var exDriverbar: Driverbar = Driverbar()
var exCar: ExCar = ExCar()

exDriverbar.delegate = exCar

exDriverbar.turnLeft()
exDriverbar.turnRight()
```

위 예시 코드를 확인해보면, `Driverbar`(`TextField` 역할), `DriverbarDelegate`(`UITextFieldDelegate` 역할), `ExCar`(`ViewController` 역할)로 이루어져 있음.

하나씩 분석해보겠음.

```swift
// UITextFieldDelegate
@objc
protocol DriverbarDelegate{
    func isTurnLeft() -> Bool
    func isTurnRight() -> Bool
    
    @objc optional func turnLeft()
    @objc optional func turnRight()
}
```

`DriverbarDelegate`는 프로토콜로 선언되어 있으며, 사용되는 메서드들이 정의되어 있음.

```swift
// TextField
class Driverbar{
    var delegate: DriverbarDelegate?
    
    func turnLeft(){
        if (delegate?.isTurnLeft())!{
            delegate?.turnLeft?()
        }
    }
    
    func turnRight(){
        if (delegate?.isTurnRight())!{
            delegate?.turnRight?()
        }
    }
}
```

`Driverbar`는 `DriverbarDelegate` 프로토콜 타입을 가진 `delegate` 변수를 옵셔널로 선언해둠.

그리고 특정 메서드를 실행하면 자신이 처리하는 것이 아닌 `DriverbarDelegate`에 선언되어 있는 메서드를 호출하여 처리함.

```swift
// ViewController
class ExCar: DriverbarDelegate{
    
    func isTurnLeft() -> Bool {
        print("ExCar isTurnLeft 호출")
        
        return true
    }
    
    func isTurnRight() -> Bool {
        print("ExCar isTurnRight 호출")
        
        return true
    }
    
    func turnLeft() {
        print("ExCar turnLeft 호출")
    }
    
    func turnRight() {
        print("ExCar turnRight 호출")
    }
}
```

`ExCar`는 `DriverbarDelegate` 프로토콜을 채택하여 프로토콜에 선언되어 있는 메서드를 구현함.

```swift
var exDriverbar: Driverbar = Driverbar()
var exCar: ExCar = ExCar()

exDriverbar.delegate = exCar
```

가장 핵심적인 부분으로 `ExCar` 인스턴스 변수를 `Driverbar` 클래스의 `delegate` 변수에 할당함.

```swift
exDriverbar.turnLeft()
exDriverbar.turnRight()
```

그 후 `turnLeft`, `turnRight` 메서드를 실행하면 `ExCar` 클래스에서 구현한 메서드들이 실행되는 것을 확인할 수 있음.

이처럼, 코드를 자신이 직접 처리하는 것이 아닌 `delegate`(대리자)에게 처리하는 것을 델리게이트 패턴이라고 함.