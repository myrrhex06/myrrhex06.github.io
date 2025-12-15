---
title: "Network - 유니캐스트(unicast)와 브로드캐스트(broadcast)란?"
date: 2025-12-13 10:45:00 +0900
categories: [CS, Network]
tags: [network, unicast, broadcase]
---

> 본 글은 [『혼자 공부하는 네트워크』](https://product.kyobobook.co.kr/detail/S000212911507)를 참고하여 개인 학습 목적으로 이해한 내용을 정리한 것입니다.

- 유니캐스트(unicast): 특정 하나의 호스트를 대상으로 1:1 메시지 전송.
  - 특정 수신지 주소를 알고 있어야만 가능함.
- 브로드캐스트(broadcast): 자신을 제외한 동일 네트워크 상에 모든 호스트에게 메시지 전송.
  - 개별 호스트의 주소는 모르더라도, 브로드캐스트 주소를 알아야 메시지 전송이 가능함.
  - 브로드캐스트로 전송되는 메시지의 범위를 브로드캐스트 도메인이라고 함.

