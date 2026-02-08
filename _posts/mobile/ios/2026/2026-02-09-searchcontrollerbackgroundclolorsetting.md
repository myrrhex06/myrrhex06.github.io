---
title: "UIKit - UISearchController 배경색, placeholder 색상 변경하기"
date: 2026-02-09 06:41:00 +0900
categories: [Mobile, iOS]
tags: [ios, uikit, searchcontroller, color, backgroundcolor, placeholder, icon, leftview]
---

`UISearchController`의 배경색을 변경해주기 위해서 아래와 같이 코드를 작성함.

`BookListViewController.swift`
```swift
import UIKit

final class BookListViewController: UIViewController {
    
    private let searchController = UISearchController()
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        setupUI()
    }
    
    private func setupUI(){
        setupSearchController()
    }
    
    private func setupSearchController(){
        
        // SearchBar TextField 백그라운드 색상 설정
        searchController.searchBar.searchTextField.backgroundColor = UIColor(named: "SearchBarBackgroundColor")

        // SearchBar Placeholder 글자 색상 설정
        searchController.searchBar.searchTextField.attributedPlaceholder = NSAttributedString(string: "책 제목 검색", attributes: [NSAttributedString.Key.foregroundColor: UIColor(named: "PlaceholderColor")])

        // SearchBar 돋보기 아이콘 색상 설정
        searchController.searchBar.searchTextField.leftView?.tintColor = UIColor(named: "PlaceholderColor")
        
        self.navigationItem.searchController = searchController
    }

}
```

결과

![image](/assets/img/searchcontrollerbackgroundcolorsetting.gif)