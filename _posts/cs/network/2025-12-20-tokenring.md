---
title: "Network - 토큰링이란?"
date: 2025-12-20 12:39:00 +0900
categories: [CS, Network]
tags: [network, tokenring, ethernet, lan]
---

> 본 글은 [『혼자 공부하는 네트워크』](https://product.kyobobook.co.kr/detail/S000212911507)를 참고하여 개인 학습 목적으로 이해한 내용을 정리한 것입니다.

이더넷과 같은 LAN 기술의 일종임.

네트워크 내 호스트들이 아래와 같이 링(고리) 형태로 연결되어 있음.

<img src="/assets/img/tokenring.png" alt="image" width="500">

호스트들은 돌아가면서 토큰을 주고 받게됨.

토큰을 가진 호스트만 메시지 송신이 가능함.

위 이미지에서는 Host 1이 토큰을 가지고 있기 때문에, Host 1만 메시지 송신이 가능하며, Host 2가 메시지를 보내고 싶어도 토큰을 받기 전까진 메시지를 송신할 수 없음.