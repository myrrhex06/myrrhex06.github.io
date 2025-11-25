---
title: "iOS - Core Data란?"
date: 2025-11-25 18:34:00 +0900
categories: [Mobile, iOS]
tags: [coredata, ios]
---

Apple Core Data 공식 문서를 기반으로 주요 내용을 정리함.

## **OverView**

**Apple Developer Document**

![image](/assets/img/coredataoverview.png)

Core Data는 앱 데이터를 오프라인에서도 저장할 수 있게 해주고, 임시 데이터 캐싱이나 단일 디바이스에서의 Undo 기능도 지원함.

iCloud를 쓰면 Core Data가 스키마를 Cloudkit 컨테이너로 자동으로 미러링해서 여러 디바이스 간 동기화도 가능함.

데이터 모델 편집기에서 타입과 관계를 정의하면 Core Data가 그에 맞는 클래스를 생성하고, 런타임에서 알아서 객체를 관리해줌.

## **Persistence**

**Apple Developer Document**

![image](/assets/img/coredatapersistence.png)

Core Data는 스토어에 어떻게 저장하는지에 대해 복잡한 부분을 추상화하여 직접적인 DB 접근 없이 Swift와 Objective-C로 데이터를 저장할 수 있게 함.

## **Undo and redo of individual and batched changes**

**Apple Developer Document**

![image](/assets/img/coredataundoandredo.png)

Core Data의 undo 관리자는 변화를 추적하며 개별, 그룹 혹은 전체 단위로 롤백시킬 수 있으며, 덕분에 앱에 undo, redo 기능을 쉽게 붙일 수 있음.

## **Background data tasks**

**Apple Developer Document**

![image](/assets/img/coredatabackgrounddatatask.png)

JSON을 객체로 파싱하는 것처럼 UI를 막을 수 있는 작업은 백그라운드에서 처리하고, 결과는 캐싱하거나 저장해서 라운드트립을 줄일 수 있음.

## **View synchronization**

**Apple Developer Document**

![image](/assets/img/coredataviewsynchronization.png)

Core Data는 TableView와 CollectionView에 바로 사용할 수 있는 데이터 소스를 제공하여 데이터와 화면이 자연스럽게 동기화하도록 도와줌.

## **Versioning and migration**

**Apple Developer Document**

![image](/assets/img/coredataversioningandmigration.png)

Core Data는 데이터 모델 버전 관리와 앱이 바뀌었을 때 사용자 데이터를 마이그레이션할 수 있는 기능을 기본으로 제공함.

## **Reference**
- [https://developer.apple.com/documentation/coredata](https://developer.apple.com/documentation/coredata)
