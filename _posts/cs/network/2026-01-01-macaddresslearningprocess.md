---
title: "Network - 스위치의 MAC 주소 학습 과정"
date: 2026-01-01 11:58:00 +0900
categories: [CS, Network]
tags: [network, switch, macaddress, learning, process, flooding, filtering, forwarding, aging]
---

> 본 글은 [『혼자 공부하는 네트워크』](https://product.kyobobook.co.kr/detail/S000212911507)를 참고하여 개인 학습 목적으로 이해한 내용을 정리한 것입니다.

<img src="/assets/img/macaddresslearning01.png" alt="image" width="500">

위 사진처럼 하나의 L2 스위치에 4개의 호스트가 연결되어 있다고 가정하고 설명하겠음.

초기에는 MAC 주소 테이블에 값이 저장되어 있지 않음.

<img src="/assets/img/macaddresslearning02.png" alt="image" width="500">

위 이미지처럼 호스트 1이 호스트 3에게 프레임을 전송하면, 스위치는 해당 포트와 전달받은 프레임의 송신지 MAC 주소를 확인하여 MAC 주소 테이블에 저장함.

하지만 수신지인 호스트 3의 MAC 주소는 아직 MAC 주소 테이블에 저장되어 있지 않음.

<img src="/assets/img/macaddresslearning03.png" alt="image" width="500">

이때에는 위 사진처럼 송신지를 제외한 포트에 연결된 모든 호스트들에게 프레임을 전송함.<br>
이렇게 송신지를 제외한 모든 포트에 프레임을 보내는 것을 **플러딩** 이라고 함.

프레임을 받은 호스트들은 각각 수신지 MAC 주소를 확인한 후 자신과 관계 없으면 폐기하고, 관계가 있다면 응답 프레임을 보내게 됨.

응답 프레임을 받은 스위치는 해당 프레임의 송신지 주소를 확인한 후 MAC 주소 테이블에 저장함.

스위치가 프레임을 전달받으면 어디로 내보내야하는지, 어디로 내보내지 말아야하는지 결정하는 작업을 **필터링**이라고 하며, 전송 해야하는 포트에 실제로 프레임을 전송하는 작업을 **포워딩**이라고 함.

마지막으로 MAC 주소 테이블에 저장되어 있는 한 호스트가 일정 시간 동안 전송받은 프레임이 없으면 이 호스트와 포트를 매핑한 정보를 지움. 이것을 **에이징**이라고 함.
