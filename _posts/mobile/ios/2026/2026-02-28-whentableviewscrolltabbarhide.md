---
title: "UIKit - 스크롤 시 TabBar 숨김 처리하기"
date: 2026-02-28 12:22:00 +0900
categories: [Mobile, iOS]
tags: [ios, uikit, uitabbarcontroller, uitableview, uiscrollview, delegate]
---

스크롤 시 `TabBar`를 숨김처리 하기 위해선 `delegate` 메서드인 `scrollViewWillEndDragging`을 구현하여 처리할 수 있음.

`velocity` 값은 아래로 스크롤 했을 경우 양수 값을, 위로 스크롤 했을 경우 음수 값을 가짐.

구현 로직
```swift
extension BookListViewController: UITableViewDataSource, UITableViewDelegate{
    
    // 중복 연산을 막기 위한 프로퍼티
    private var isHiddenTabBar: Bool = false

    func scrollViewWillEndDragging(
        _ scrollView: UIScrollView,
        withVelocity velocity: CGPoint,
        targetContentOffset: UnsafeMutablePointer<CGPoint>
    ) {
        // TabBar height 값 가져오기
        let height = self.tabBarController?.tabBar.frame.height ?? 0
        
        UIView.animate(withDuration: 0.5) {
            // 위로 스크롤 했으며 TabBar 숨김 처리가 되어 있는 상태일 경우 처리
            if velocity.y < 0 && self.isHiddenTabBar{
                // 투명도 조정
                self.tabBarController?.tabBar.alpha = 1.0

                // TabBar의 origin 값을 조정하여 화면 위로 다시 나타나도록 처리
                self.tabBarController?.tabBar.frame.origin = CGPoint(x: 0, y: UIScreen.main.bounds.maxY - height)

                // view의 origin y 값에 TabBar의 높이 값을 빼줘서 TabBar가 렌더링될 영역을 확보함 
                self.view.frame.origin = CGPoint(x: self.view.frame.origin.x, y: self.view.frame.origin.y - height)

                // TabBar 숨김 처리 false
                self.isHiddenTabBar = false
            }
            // 아래로 스크롤 했으며 TabBar 숨김 처리가 되어 있지 않은 상태일 경우 처리
            else if velocity.y > 0 && !self.isHiddenTabBar{
                // 투명도 조정
                self.tabBarController?.tabBar.alpha = 0.0

                // TabBar의 origin 값을 조정하여 화면 밖으로 내려줌
                self.tabBarController?.tabBar.frame.origin = CGPoint(x: 0, y: UIScreen.main.bounds.maxY)

                // TabBar가 차지하고 있는 영역이 없어졌기 때문에 view의 origin y에 TabBar의 높이 값을 더해 빈 공간을 채워줌
                self.view.frame.origin = CGPoint(x: self.view.frame.origin.x, y: self.view.frame.origin.y + height)

                // TabBar 숨김 처리 true
                self.isHiddenTabBar = true
            }
        }
    }
}
```

결과 

![image](/assets/img/whentableviewscrolltabbarhideresult.gif)

## **Reference**
- [https://ios-development.tistory.com/734](https://ios-development.tistory.com/734)
- [https://velog.io/@yimkeul/UIKit-%ED%95%98%EB%8B%A8-%ED%83%AD%EB%B0%94-%EC%8A%A4%ED%81%AC%EB%A1%A4-%EB%B0%A9%ED%96%A5%EC%97%90-%EB%94%B0%EB%A5%B8-%EC%88%A8%EA%B8%B0%EA%B8%B0%EB%B3%B4%EC%9D%B4%EA%B8%B0-%EB%B0%A9%EB%B2%95](https://velog.io/@yimkeul/UIKit-%ED%95%98%EB%8B%A8-%ED%83%AD%EB%B0%94-%EC%8A%A4%ED%81%AC%EB%A1%A4-%EB%B0%A9%ED%96%A5%EC%97%90-%EB%94%B0%EB%A5%B8-%EC%88%A8%EA%B8%B0%EA%B8%B0%EB%B3%B4%EC%9D%B4%EA%B8%B0-%EB%B0%A9%EB%B2%95)