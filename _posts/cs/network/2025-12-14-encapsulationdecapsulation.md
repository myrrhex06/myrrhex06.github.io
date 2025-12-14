---
title: "Network - 캡슐화(Encapsulation)와 역캡슐화(Decapsulation)란?"
date: 2025-12-14 11:21:00 +0900
categories: [CS, Network]
tags: [network, encapsulation, decapsulation]
---

현대 네트워크 통신은 패킷 교환 방식으로 구성되어 있음.<br>

> 패킷 교환 방식은 [이글](https://myrrhex06.github.io/posts/exchangepacketcircuit) 참고

패킷을 송수신할 때는 캡슐화/역캡슐화 과정을 거치게됨.

## **캡슐화란?**

송신 과정에서 패킷에 헤더와 트레일러를 추가하는 과정을 의미함.

<img src="/assets/img/encapsulation.png" alt="image" width="500">

하위 계층은 상위 계층에서 내려온 패킷을 페이로드 삼아 프로토콜에 맞는 헤더(또는 트레일러)를 붙이게 됨.

> 데이터 링크 계층에서는 충돌 문제가 났는지 확인하기 위해 트레일러를 붙이게 됨.
{: .prompt-tip }

## **역캡슐화**

수신 과정에서 패킷에 헤더와 트레일러를 제거하는 과정을 의미함.

<img src="/assets/img/decapsulation.png" alt="image" width="500">