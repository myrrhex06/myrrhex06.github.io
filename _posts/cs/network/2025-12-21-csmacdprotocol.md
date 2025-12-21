---
title: "Network - CSMA/CD 프로토콜이란?"
date: 2025-12-21 11:57:00 +0900
categories: [CS, Network]
tags: [network, hub, protocol, csmacd]
---

> 본 글은 [『혼자 공부하는 네트워크』](https://product.kyobobook.co.kr/detail/S000212911507)를 참고하여 개인 학습 목적으로 이해한 내용을 정리한 것입니다.

CSMA/CD 프로토콜이란 Carrier Sense Multiple Access with Collision Detection의 약자로, 반이중 통신에서 발생하는 충돌 문제를 해결하기 위해 사용하는 프로토콜임.

### **캐리어 감지(Carrier Sense)**
- 메시지를 송신하기 전 현재 네트워크에서 전송중인 것이 있는지 확인함.
- 현재 통신 매체 사용 가능 여부를 검증하는 것.

### **다중 접근(Multiple Access)**
- 다수의 호스트가 네트워크에 접근하는 상황을 의미함.
  - 이때 충돌이 발생하게 됨.

### **충돌 검출(Collision Detection)**
- 충돌이 발생하면 이를 검출함.
- 충돌을 검출한 호스트는 전송을 중단하고 다른 호스트들에게 충돌이 발생했음을 알리는 잼 신호를 보내게 됨.
- 전송을 중단한 호스트는 임의의 시간을 기다린 뒤 재전송함.
