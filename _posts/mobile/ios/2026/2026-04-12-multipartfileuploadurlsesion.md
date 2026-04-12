---
title: "UIKit - Multipart File 업로드 요청 구현하기"
date: 2026-04-12 21:15:00 +0900
categories: [Mobile, iOS]
tags: [ios, uikit, urlsession, multipart, formdata, image, http]
---

## **multipart/form-data란?**

`multipart/form-data`는 서버로 파일이나 대용량 데이터를 전송할 때 사용하는 HTTP Content-Type임.

`boundary`로 경계선을 그으며, 경계선을 기준으로 나눠진 데이터 조각들을 `part`라고 함.

`multipart/form-data`를 사용하면 하나의 요청에서 텍스트 데이터와 파일 모두 전송이 가능함.

HTTP 요청 body 예시
```
POST /upload HTTP/1.1
Host: example.com
Content-Type: multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW

------WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="username"

david
------WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="age"

20
------WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="file"; filename="profile.png"
Content-Type: image/png

(binary data...)
------WebKitFormBoundary7MA4YWxkTrZu0gW--
```

`boundary`를 통해 파트를 구분짓고 있으며, 각 파트에는 요청을 보낼 데이터 정보가 들어가있음.

`--boundary`로 각 파트의 시작을 알리고 있으며, 모든 파트 기입이 끝났다면 `--boundary--`로 끝났음을 명시해줘야함.

## **URLSession을 통해 구현하기**

`URLSession`을 통해 `multipart/form-data`를 구현하기 위해서는 `body`를 직접 만들어줘야함.

> `Alamofire`같은 오픈소스를 사용하면 훨씬 수월하게 구현이 가능함.
{: .prompt-tip }

파일 업로드를 처리할 때는 `URLSession`으로 API를 호출할 때 일반적으로 사용하는 `dataTask`가 아닌 `uploadTask`를 사용할 것임.

### **uploadTask**

`uploadTask`는 이름에서 알 수 있듯이 파일 전송이나 대용량 데이터 업로드에 특화되어 있음.

백그라운드 세션을 지원하기 때문에 업로드 중 앱을 종료하더라도 업로드 처리가 가능함.

> `dataTask`는 백그라운드 세션을 지원하지 않음.
{: .prompt-tip }

예시
```swift
import UIKit

final class FileService{

    public func upload(data: Data, completionHandler: @escaping (Result<UploadFileResponse, NetworkingError>) -> Void){
        
        // boundary 값은 요청 내에서 고유해야하기 때문에 UUID를 통해 생성
        let uuid = UUID().uuidString
        
        var request = URLRequest(url: url)
        
        request.httpMethod = "POST"

        // multipart/form-data 헤더 설정
        request.setValue("multipart/form-data; boundary=\(uuid)", forHTTPHeaderField: "Content-Type")
        
        // body 설정
        var body = Data()

        // CRLF(\r\n)로 헤더와 데이터 구분
        body.append("--\(uuid)\r\n".data(using: .utf8)!)
        body.append("Content-Disposition: form-data; name=\"file\"; filename=\"image.jpg\"\r\n".data(using: .utf8)!)
        body.append("Content-Type: image/jpeg\r\n\r\n".data(using: .utf8)!)
        body.append(data)
        body.append("\r\n".data(using: .utf8)!)
        body.append("--\(uuid)--\r\n".data(using: .utf8)!)
        
        // URLSession uploadTask를 통해 파일 업로드 처리
        urlSession.uploadTask(with: request, from: body) { data, response, error in

            ...

        }.resume()
    }

}
```

## **Reference**
- [https://tantandero.tistory.com/66](https://tantandero.tistory.com/66)
- [https://velog.io/@shin6403/HTTP-multipartform-data-%EB%9E%80](https://velog.io/@shin6403/HTTP-multipartform-data-%EB%9E%80)
- [https://ios-development.tistory.com/1419](https://ios-development.tistory.com/1419)
- [https://wody.tistory.com/16](https://wody.tistory.com/16)
- [https://sandclock-itblog.tistory.com/244](https://sandclock-itblog.tistory.com/244)
