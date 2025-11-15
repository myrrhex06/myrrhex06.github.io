---
title: "UIKit - UICollectionView 개념 및 사용 방법"
date: 2025-11-15 18:14:00 +0900
categories: [Mobile, iOS]
tags: [uikit, code, uicollectionview, collectionview, collectionviewcell, json, networking, uiimageview]
---

## **UICollectionView란?**

`UICollectionView`는 `UITableView`와 비슷하지만 커스텀 레이아웃을 통해 `UItableView`보다 더 다양한 화면을 구성할 수 있음.

`UITableView`와 유사하게 셀을 등록해주고, `DataSource`, `Delegate`를 통해 셀의 개수, 사용자 동작 처리가 가능함.

차이점으로는 `UICollectionView`에는 `Layout`을 반드시 설정해줘야만함.

> `UITableView`와는 달리 `Layout`을 반드시 설정해줘야하며, 설정하지 않을 경우 에러가 발생함.
{: .prompt-tip }

## **구현 예시**
iTunes Search API를 통해 이미지를 가져와 `CollectionView`로 표시하는 예시는 아래와 같음.

> iTunes Search API Document: [link](https://developer.apple.com/library/archive/documentation/AudioVideo/Conceptual/iTuneSearchAPI/index.html#//apple_ref/doc/uid/TP40017632-CH3-SW1)
{: .prompt-tip }

`ViewController.swif`
```swift
import UIKit

class ViewController: UIViewController {
    
    private let urlSession = URLSession.shared
    
    // CollectionView 셀 수
    private let columnCount: CGFloat = 3
    
    // CollectionView 간격 크기
    private let columnSpacing: CGFloat = 10
    
    let collectionView: UICollectionView = {
        
        // CollectionView Layout 인스턴스 생성
        let layout = UICollectionViewFlowLayout()
        
        // CollectionView 스크롤 방향 설정
        layout.scrollDirection = .vertical
        
        return UICollectionView(frame: .zero, collectionViewLayout: layout) // CollectionView 생성
    }()
    
    // 데이터 배열
    var dataArray: [Software] = []

    override func viewDidLoad() {
        super.viewDidLoad()
        
        // 초기 데이터 세티
        fetchData { result in
            
            switch result{
            case .success(let data):
                
                guard let array = data else { return }
                
                self.dataArray = array
                print("dataArray count : \(self.dataArray.count)")
                
                DispatchQueue.main.async {
                    // CollectionView 데이터 로드
                    self.collectionView.reloadData()
                }
               
                
            case .failure(let error):
                print(error.localizedDescription)
            }
        }
        
        // CollectionView 설정
        setupCollectionView()
    }
    
    func setupCollectionView(){
        
        // 오토 레이아웃 설정
        setupAutoLayout()
        
        // CollectionViewCell 등록
        collectionView.register(CustomCollectionViewCell.self, forCellWithReuseIdentifier: "CustomCollectionViewCell")
        
        // CollectionView DataSource 설정
        collectionView.dataSource = self
        
        // CollectionView Delegate 설정
        collectionView.delegate = self
    }
    
    func setupAutoLayout(){
        view.addSubview(collectionView)
        
        collectionView.translatesAutoresizingMaskIntoConstraints = false
        
        NSLayoutConstraint.activate([
            collectionView.leadingAnchor.constraint(equalTo: view.leadingAnchor),
            collectionView.trailingAnchor.constraint(equalTo: view.trailingAnchor),
            collectionView.bottomAnchor.constraint(equalTo: view.bottomAnchor),
            collectionView.topAnchor.constraint(equalTo: view.topAnchor)
        ])
    }
}

// MARK: - CollectionView DataSource
extension ViewController: UICollectionViewDataSource{
    
    // CollectionView 요소 수 반환
    func collectionView(_ collectionView: UICollectionView, numberOfItemsInSection section: Int) -> Int {
        
        print("count : \(dataArray.count)")
        
        return dataArray.count
    }
    
    // CollectionView 셀 반환
    func collectionView(_ collectionView: UICollectionView, cellForItemAt indexPath: IndexPath) -> UICollectionViewCell {
        
        let cell = collectionView.dequeueReusableCell(withReuseIdentifier: "CustomCollectionViewCell", for: indexPath)
            as! CustomCollectionViewCell
        
        cell.image = dataArray[indexPath.row].imageUrl
        
        return cell
    }
    
    // iTunes Search API 호출
    func fetchData(completionHandler: @escaping (Result<[Software]?, Error>) -> Void){
        
        let urlStr = "https://itunes.apple.com/search?term=app&media=software"
        print("urlStr : \(urlStr)")
        
        guard let url = URL(string: urlStr) else {
            print("URL 변환 실패")
            return
        }
        
        URLSession.shared.dataTask(with: url) { data, _, error in
            if error != nil{
                print("에러 발생")
                completionHandler(.failure(Error.self as! Error))
                return
            }
            
            guard let data = data else {
                print("데이터 요청 실패")
                completionHandler(.failure(Error.self as! Error))
                return
            }
            
            guard let softwareResult = try? JSONDecoder().decode(SoftwareData.self, from: data) else {
                print("파싱 실패")
                completionHandler(.failure(Error.self as! Error))
                return
            }
            
            completionHandler(.success(softwareResult.results))
        }.resume()
    }
}

// MARK: - CollectionView Layout Delegate
extension ViewController: UICollectionViewDelegateFlowLayout{
    
    // Cell 크기 설정
    func collectionView(_ collectionView: UICollectionView, layout collectionViewLayout: UICollectionViewLayout, sizeForItemAt indexPath: IndexPath) -> CGSize {

        // 전체 간격 사이즈
        let totalSpacing = columnSpacing * (columnCount - 1)
        
        // 간격 사이즈를 제외한 전체 화면 크기
        let viewWidth = self.view.frame.width - totalSpacing
        
        // 전체 화면 크기를 한 줄에 들어갈 셀 개수로 나누기 = 셀 하나의 크기
        let cellWidth = viewWidth / columnCount
        
        return CGSize(width: cellWidth, height: cellWidth)
    }
    
    // Verticle 간격 설정
    func collectionView(_ collectionView: UICollectionView, layout collectionViewLayout: UICollectionViewLayout, minimumLineSpacingForSectionAt section: Int) -> CGFloat {
    
        return self.columnSpacing
    }
    
    // Horizontal 간격 설정
    func collectionView(_ collectionView: UICollectionView, layout collectionViewLayout: UICollectionViewLayout, minimumInteritemSpacingForSectionAt section: Int) -> CGFloat {
        
        return self.columnSpacing
    }
}
```

`Software.swift`
```swift
import Foundation

struct SoftwareData: Codable{
    var resultCount: Int
    var results: [Software]
}

struct Software: Codable{
    
    var imageUrl: String?
    
    enum CodingKeys: String, CodingKey {
        case imageUrl = "artworkUrl100"
    }
}
```

결과

![image](/assets/img/collectionviewimplementresult.gif)