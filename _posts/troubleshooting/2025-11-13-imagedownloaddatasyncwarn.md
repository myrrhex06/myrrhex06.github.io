---
title: "Synchronous URL loading of ... Please switch to an asynchronous networking API such as URLSession."
date: 2025-11-13 20:35:00 +0900
categories: [Troubleshooting]
tags: [uikit, urlsession, data, image, networking]
---

## **에러**
`URLSession`과 이미지 `url`을 통해 이미지 다운로드를 구현하는 방법 외에 `Data` 구조체의 생성자를 이용하여 이미지 다운로드를 구현하는 것을 테스트해보던 도중 아래와 같은 에러가 발생함.

![image](/assets/img/imagedownloaddatasyncwarn.png)

## **원인**
위 코드는 메인 쓰레드에서 처리하고 있으며, `Data(contentsOf: url)` 생성자는 동기 방식으로 처리되기 때문에 이미지 다운로드 같은 시간이 많이 소요되는 작업을 메인 쓰레드에서 동기적으로 처리하고 있기 때문에 경고 메시지를 보내는 것임.

## **해결**

```swift
...
    func downloadImage(){
        guard let urlString = previewUrl, let url = URL(string: urlString) else { return }
        
        DispatchQueue.global().async {
            guard let data = try? Data(contentsOf: url) else { return }
            
            // 테이블뷰 셀 이미지 덮어쓰기 방지용
            guard self.previewUrl == url.absoluteString else { return }
            
            DispatchQueue.main.async {
                self.mainImageView.image = UIImage(data: data)
            }
        }
    }
```

`DispatchQueue`를 통해 메인 쓰레드가 아닌 다른 쓰레드에서 비동기적으로 처리하도록 하였으며, 화면을 업데이트 하는 것은 메인 쓰레드에서 처리해야하기 때문에 화면 업데이트 처리 로직만 메인 쓰레드로 감싸서 해결함.
