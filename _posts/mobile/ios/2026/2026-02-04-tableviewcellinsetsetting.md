---
title: "UIKit - UITableViewCell 간격 설정하기"
date: 2026-02-04 06:44:00 +0900
categories: [Mobile, iOS]
tags: [ios, uikit, tableview, cell, inset, edges]
---

`TableViewCell`에 간격을 주는 방법으로는 `layoutSubviews` 메서드를 오버라이딩 하여 처리할 수 있음.

예시 코드
```swift
import UIKit

final class BookListTableViewCell: UITableViewCell {

    override func setSelected(_ selected: Bool, animated: Bool) {
        super.setSelected(selected, animated: animated)
        
        setupUI()
    }

    override func layoutSubviews() {
        super.layoutSubviews()
        // 간격 설정
        contentView.frame = contentView.frame.inset(by: UIEdgeInsets(top: 10, left: 10, bottom: 10, right: 10))

        // corner radius 설정
        contentView.layer.cornerRadius = 10
        contentView.clipsToBounds = true
    }

    private func setupUI(){
      // TableViewCell에 표시할 View들을 contentView에 추가
        self.contentView.addSubview(stackView)
        self.contentView.backgroundColor = .red
        
        leftImageView.snp.makeConstraints { make in
            make.width.equalTo(80)
        }
        
        stackView.snp.makeConstraints { make in
            make.edges.equalToSuperview().inset(10)
        }
    }
}
```

결과

![image](/assets/img/tableviewcellinsetsetting.gif)

## **Reference**
- [https://pooh-footprints.tistory.com/entry/Swift-UITableViewCell%EC%9D%98-%EA%B0%84%EA%B2%A9%EC%9D%84-%EC%84%A4%EC%A0%95%ED%95%98%EB%8A%94-%EB%B0%A9%EB%B2%95#google_vignette](https://pooh-footprints.tistory.com/entry/Swift-UITableViewCell%EC%9D%98-%EA%B0%84%EA%B2%A9%EC%9D%84-%EC%84%A4%EC%A0%95%ED%95%98%EB%8A%94-%EB%B0%A9%EB%B2%95#google_vignette)