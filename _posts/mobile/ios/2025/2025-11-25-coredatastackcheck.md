---
title: "iOS - Core Data stack이란?"
date: 2025-11-25 20:08:00 +0900
categories: [Mobile, iOS]
tags: [coredata, ios, coredatastack, model, context, storecoordinator]
---

Apple Core Data stack 공식 문서를 기반으로 주요 내용을 정리함.

## **Overview**

**Apple Developer Document**

![image](/assets/img/coredatastackoverview.png)

Core Data는 앱의 모델 레이어를 구성하는 핵심 클래스들을 제공함.

- `NSManagedObjectModel`: 데이터 타입(엔티티), 각 속성, 관계 구조를 정의한 모델 정보
- `NSManagedObjectContext`: 엔티티 인스턴스들의 생성, 수정, 삭제 같은 변경 사항을 추적
- `NSPersistentStoreCoordinator`: 컨텍스트가 요청한 저장, 조회 작업을 실제 저장소에 읽고 쓰는 역할
- `NSPersistentContainer`: 모델, 컨텍스트, 스토어 코디네이터를 한번에 구성해줌

## **Reference**
- [https://developer.apple.com/documentation/coredata/core_data_stack](https://developer.apple.com/documentation/coredata/core_data_stack)