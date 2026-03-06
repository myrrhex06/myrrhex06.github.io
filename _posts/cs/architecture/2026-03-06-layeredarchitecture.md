---
title: Architecture - 레이어드 아키텍처(Layered Architecture)란?
date: 2026-03-06 17:34:00 +0900
categories: [CS, Architecture]
tags: [architecture, layeredarchitecture, layer]
---

레이어드 아키텍처란 각 관심사 별로 계층을 분리한 아키텍처를 의미함.<br>
각 계층이 특정 역할, 책임을 맡고 구분이 명확함.

계층 간 통신은 추상화된 인터페이스를 통해 이루어지며 의존성은 항상 상위 계층에서 하위 계층으로만 흐르는 단방향 의존 관계를 가짐.

레이어드 아키텍처에 가장 일반적인 형태는 4-Tier 아키텍처임. 
- **Presentation Layer**: 사용자, 클라이언트 시스템과 직접적으로 맞닿아 있는 부분
    - `Controller`
- **Business Layer**: 시스템의 핵심 비즈니스 규칙과 도메인 로직이 위치함
    - `Service`, `Entity`
- **Persistence Layer**: DB 상호작용을 추상화하여 데이터의 영구 저장과 관리를 담당
    - `Repository`
- **Database Layer**: 실제 데이터가 저장되는 데이터베이스 계층
    - RDB

레이어드 아키텍처의 단점은 `Service` 레이어가 `Entity`를 강하게 의존하기 때문에 테이블의 구조가 변경되면 `Service` 레이어의 로직이 함께 변경되어야함.