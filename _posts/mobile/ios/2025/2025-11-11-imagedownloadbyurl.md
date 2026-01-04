---
title: "UIKit - URL로 이미지 다운로드하고 표시하기"
date: 2025-11-11 18:23:00 +0900
categories: [Mobile, iOS]
tags: [uikit, code, urlsession, dispatchqueue, uiimageview, uiimage]
---

`URLSession`을 사용하여 HTTP 요청을 보내 이미지 바이너리 데이터를 다운로드 받은 후 `UIImage`로 변환하여 처리 가능함.

구현 예시
```swift
import UIKit

final class ViewController: UIViewController {
    
    // MARK: - 이미지 뷰
    private let imageView: UIImageView = {
        let view = UIImageView()
        
        view.backgroundColor = .gray
        
        view.translatesAutoresizingMaskIntoConstraints = false
        
        return view
    }()
    
    // MARK: - 다운로드 버튼
    private lazy var downloadButton: UIButton = {
        let btn = UIButton()
        
        btn.setTitle("다운로드", for: .normal)
        btn.backgroundColor = .black
        btn.setTitleColor(.white, for: .normal)
        btn.titleLabel?.font = UIFont.boldSystemFont(ofSize: 20)
        
        btn.addTarget(self, action: #selector(downloadImage), for: .touchUpInside)
        
        btn.translatesAutoresizingMaskIntoConstraints = false
        
        return btn
    }()
    
    // iTunes API 응답값으로 받은 jpg URL
    private let urlStr: String = "https://is1-ssl.mzstatic.com/image/thumb/Music115/v4/ab/2e/3a/ab2e3a42-1c8b-9f3f-3d36-da74de7be595/886449538959.jpg/60x60bb.jpg"

    // MARK: - 화면 구성
    override func viewDidLoad() {
        super.viewDidLoad()
        
        setupImageView()
        setupDownloadButton()
    }
    
    override func viewDidLayoutSubviews() {
        super.viewDidLayoutSubviews()
        
        imageView.layer.cornerRadius = imageView.frame.width / 2.0
        imageView.clipsToBounds = true
    }
    
    func setupImageView(){
        view.addSubview(imageView)
        
        NSLayoutConstraint.activate([
            imageView.centerYAnchor.constraint(equalTo: view.centerYAnchor),
            imageView.centerXAnchor.constraint(equalTo: view.centerXAnchor),
            imageView.heightAnchor.constraint(equalToConstant: 200),
            imageView.widthAnchor.constraint(equalToConstant: 200)
        ])
    }
    
    func setupDownloadButton(){
        view.addSubview(downloadButton)
        
        NSLayoutConstraint.activate([
            downloadButton.centerXAnchor.constraint(equalTo: view.centerXAnchor),
            downloadButton.topAnchor.constraint(equalTo: imageView.bottomAnchor, constant: 30),
            downloadButton.heightAnchor.constraint(equalToConstant: 50),
            downloadButton.widthAnchor.constraint(equalToConstant: 130)
        ])
    }
    
    // MARK: - 이미지 다운로드
    @objc func downloadImage(){
        guard let url = URL(string: urlStr)
        else {
            return
        }
        
        // HTTP 요청
        URLSession.shared.dataTask(with: url) { data, response, error in
            guard error == nil else {
                print(error!)
                return
            }
            
            guard let data = data else {
                print("데이터 nil")
                return
            }
            
            DispatchQueue.main.async {
                self.imageView.image = UIImage(data: data) // 응답값으로 받은 이미지 UIImage로 변환
            }
        }.resume()
    }
}
```

> 화면을 그리는 역할은 오직 메인 쓰레드에서만 가능하기 때문에 `DispatchQueue`를 통해 메인 쓰레드에 접근하여 화면을 다시 그리도록 처리함.
{: .prompt-tip }

결과 

![image](/assets/img/imagedownloadbyurlresult.gif)