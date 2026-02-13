---
title: "Troubleshooting - Capture of 'completion' with non-sendable type '(RegisterResult?) -> Void' in a @Sendable closure 경고"
date: 2025-11-04 18:51:00 +0900
categories: [Troubleshooting]
tags: [uikit, swift, urlsession, closure, concurrency, escaping]
---

## **에러**
URLSession을 통해 네트워킹을 처리하는 것을 공부하던 중 아래와 같은 경고가 나타남.

발생 경고
![image](/assets/img/escapingwarnningcheck.png)

## **원인**
구현했던 코드
```swift
final class AuthService{
    
    let session = URLSession.shared
    
    func fetchRegister(data: RegisterRequest, completion: @escaping (RegisterResult?) -> Void){
        
        var request = URLRequest(url: URL(string: "http://localhost:9000/api/v1/auth/register")!)
        
        let body = try? JSONEncoder().encode(data)
        
        request.httpMethod = "POST"
        request.addValue("application/json", forHTTPHeaderField: "Content-Type")
        request.httpBody = body
        
        let task = session.dataTask(with: request) { data, response, error in

            if let error = error{
                completion(nil)
                return
            }
            
            if let data = data{
                guard let result = try? JSONDecoder().decode(ResponseWrapper<RegisterResult>.self, from: data) else{
                    completion(nil)
                    return
                }
                
                completion(result.result)
            }
        }
        
        task.resume()
    }
}
```

`dataTask` 메서드는 비동기적으로 실행되는데, 이것은 메인 쓰레드가 아닌 백그라운드 쓰레드가 접근할 수 있고 여러 쓰레드가 동시에 접근할 수도 있다는 뜻임. 

이런 이유 때문에 매개 변수로 받는 `completionHandler`는 자동으로 Swift 컴파일러에서 `@Sendable` 클로저로 간주함.

> `@Sendable` 클로저란 여러 스레드나 동시성 컨텍스트에서 안전하게 공유되거나 접근될 수 있는 타입에 클로저를 뜻함.
{: .prompt-tip }

`@escaping`이 붙어 있는 `completion`은 `@Sendable`로 처리되지 않음.

```swift
let task = session.dataTask(with: request) { data, response, error in
    if let error = error {
        completion(nil)
        return
    }

    if let data = data {
        guard let result = try? JSONDecoder().decode(ResponseWrapper<RegisterResult>.self, from: data) else {
            completion(nil)
            return
        }

        completion(result.result)
    }
}
```

`dataTask`의 `@Sendable`로 간주되는 클로저 내부에서 `escaping` 타입의 클로저를 호출하고 있음.

이때, 컴파일러는 여러 쓰레드에서 호출될 수 있는 `dataTask` 메서드에서 Thread-safe 하지 않은 `completion` 클로저를 호출하는 것에 경고를 하는 것임.