---
title: "UIKit - Refresh Token 만료 시 로그인 화면 전환 처리하기"
date: 2026-06-05 06:49:00 +0900
categories: [Mobile, iOS]
tags: [ios, uikit, notificationcenter, name, refreshtoken, jwt, scenedelegate, delegate, event]
---

사용자가 로그인하여 발급받은 Access Token이 만료되었을 경우 같이 발급된 Refresh Token을 통해 재발급 처리를 위해 공통 요청 객체를 아래와 같이 구성해뒀음.

`NetworkingWrapper.swift`
```swift
import UIKit

final class NetworkingWrapper {

    public static let shared = NetworkingWrapper()
    
    private let authService: AuthService = AuthService.shared
    private let session: URLSession = URLSession(configuration: .default)
    
    private var isLoading: Bool = false

    private init() {}
    
    public func request(
        urlRequest: URLRequest,
        completionHandler: @escaping (Data?, URLResponse?, Error?) -> Void
    ) {
        var request = urlRequest

        // Access, Refresh Token read
        guard let accessToken = KeyChainUtil.read(key: Constants.ACCESS_TOKEN),
            let refreshToken = KeyChainUtil.read(key: Constants.REFRESH_TOKEN)
        else { return }
        
        // Bearer Header 설정
        request.setValue(
            "Bearer \(accessToken)", forHTTPHeaderField: "Authorization")

        // HTTP 요청
        session.dataTask(with: request) { data, response, error in

            // HTTP Status가 403인 경우
            if let httpResponse = response as? HTTPURLResponse,
               httpResponse.statusCode == 403
            {
                if self.isLoading { return }
                
                self.isLoading = true

                // 토큰 재발급 API 호출
                self.authService.tokenRefresh(refreshRequest: RefreshTokenRequest(refreshToken: refreshToken)) { result in
                    switch result {
                    case .success(let response):
                        // 성공 시 토큰을 업데이트한 뒤 원래 요청을 다시 수행

                        guard let response = response else {
                            self.isLoading = false
                            return
                        }
                        
                        KeyChainUtil.create(key: Constants.ACCESS_TOKEN, data: response.accessToken)
                        KeyChainUtil.create(key: Constants.REFRESH_TOKEN, data: response.refreshToken)
                        
                        self.request(urlRequest: request, completionHandler: completionHandler)
                        
                    case .failure(let error):
                        print("error :\(error)")
                    }
                    
                    self.isLoading = false
                }
                
                return
            }
            
            completionHandler(data, response, error)
        }.resume()
    }
}
```

Refresh Token이 만료되었을 경우에는 로그인을 다시 해줘야하기 때문에 로그인 화면으로 전환할 수 있도록 처리해줘야했음.

토큰 재발급을 실패할 경우 해당 API를 호출하는 화면이라면 전부 에러를 반환받을 수 있기 때문에 전역적으로 관리가 필요했음.

`Delegate`나 `Closure`처럼 특정 객체에만 이벤트를 보내는 것보다는 `NotificationCenter`를 통해 `Notification`을 보내 처리하는 것이 더 적절하다고 생각했음.

또한, `rootViewController`에 접근이 가능한 `SceneDelegate` 내에서 로직을 구현하기로 결정함.

`NotificationCenterName+TokenDeleted`
```swift
import Foundation

extension Notification.Name {
    static let tokenDeleted = Notification.Name("tokenDeleted")
}
```

`NetworkingWrapper.swift`
```swift
import UIKit

final class NetworkingWrapper {

    public static let shared = NetworkingWrapper()
    
    private let authService: AuthService = AuthService.shared
    private let session: URLSession = URLSession(configuration: .default)
    
    private var isLoading: Bool = false

    private init() {}
    
    public func request(
        urlRequest: URLRequest,
        completionHandler: @escaping (Data?, URLResponse?, Error?) -> Void
    ) {
        var request = urlRequest

        // Access, Refresh Token read
        guard let accessToken = KeyChainUtil.read(key: Constants.ACCESS_TOKEN),
            let refreshToken = KeyChainUtil.read(key: Constants.REFRESH_TOKEN)
        else { return }
        
        // Bearer Header 설정
        request.setValue(
            "Bearer \(accessToken)", forHTTPHeaderField: "Authorization")

        // HTTP 요청
        session.dataTask(with: request) { data, response, error in

            // HTTP Status가 403인 경우
            if let httpResponse = response as? HTTPURLResponse,
               httpResponse.statusCode == 403
            {
                if self.isLoading { return }
                
                self.isLoading = true

                // 토큰 재발급 API 호출
                self.authService.tokenRefresh(refreshRequest: RefreshTokenRequest(refreshToken: refreshToken)) { result in
                    switch result {
                    case .success(let response):
                        // 성공 시 토큰을 업데이트한 뒤 원래 요청을 다시 수행

                        guard let response = response else {
                            self.isLoading = false
                            return
                        }
                        
                        KeyChainUtil.create(key: Constants.ACCESS_TOKEN, data: response.accessToken)
                        KeyChainUtil.create(key: Constants.REFRESH_TOKEN, data: response.refreshToken)
                        
                        self.request(urlRequest: request, completionHandler: completionHandler)
                        
                    case .failure(let error):
                        // 에러 발생 시 토큰 제거 및 로그인 화면 전환 
                        KeyChainUtil.delete(key: Constants.ACCESS_TOKEN)
                        KeyChainUtil.delete(key: Constants.REFRESH_TOKEN)
                        
                        NotificationCenter.default.post(name: .tokenDeleted, object: nil)
                    }
                    
                    self.isLoading = false
                }
                
                return
            }
            
            completionHandler(data, response, error)
        }.resume()
    }
}
```

`SceneDelegate.swift`
```swift
import UIKit

class SceneDelegate: UIResponder, UIWindowSceneDelegate {

    var window: UIWindow?

    func scene(_ scene: UIScene, willConnectTo session: UISceneSession, options connectionOptions: UIScene.ConnectionOptions) {
        
        // NotificationCenter Observer 등록
        NotificationCenter.default.addObserver(self, selector: #selector(tokenDeleted), name: .tokenDeleted, object: nil)
    }

    func changeRootViewController(viewController: UIViewController){
        guard let window = window else { return }
        
        UIView.transition(with: window, duration: 0.3, options: .transitionCrossDissolve) {
            window.rootViewController = viewController
            window.makeKeyAndVisible()
        }
    }

    @objc private func tokenDeleted() {
        DispatchQueue.main.async {
            self.changeRootViewController(viewController: UINavigationController(rootViewController: LoginViewController()))
        }
    }

}
```