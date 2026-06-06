---
title: "Network - IP(Internet Protocol)란?"
date: 2026-05-30 17:11:00 +0900
categories: [CS, Network]
tags: [network, ip, address, fragment]
---

> 본 글은 [『혼자 공부하는 네트워크』](https://product.kyobobook.co.kr/detail/S000212911507)를 참고하여 개인 학습 목적으로 이해한 내용을 정리한 것입니다.

IP(Internet Protocol)란 네트워크 계층에서 사용되는 프로토콜 중 하나임.

IP는 IPv4, IPv6 두 가지 버전으로 나뉨.

## **주요 제공 기능**

### **IP 주소 지정**

IP 주소를 통해 전송하려는 패킷의 송수신지 주소를 지정하는 것이 가능함.

### **IP 단편화**

전송하려는 패킷의 크기가 MTU보다 클 경우 해당 패킷을 여러 패킷으로 분할하여 전송함. 수신지에 도착하면 다시 원래 패킷으로 재조립됨.

> MTU(Maximum Transmission Unit)란 IP 패킷 최대 전송 가능 크기이며 일반적으로는 1500 바이트로 잡혀있음.
{: .prompt-tip }

## **IPv4**

IPv4 주소는 4바이트(32비트)로 표현되며 8비트(0 ~ 255) 10진수 4개로 주소가 표현됨.

각 10진수들은 점(.)으로 구분되며 구분된 10진수들을 옥텟(octet)이라고 표현함.

주소 예시
```
127.0.0.1
```

### **패킷 구조**

IPv4에서 사용하는 패킷 구조는 아래와 같음.

<img src="/assets/img/ipv4_packet_structure.png" alt="image" width="500">

**주요 필드**
- 식별자(Identification): 원본 패킷을 식별하기 위한 값
- 플래그(Flags): 3개의 비트로 구성되어 있음.
  - 첫번째 비트: 현재 사용되지 않음. ( 항상 0으로 고정 )
  - 두번째 비트: DF(Don't Fragment) 비트로, 단편화 사용 여부를 나타냄.
    - 0: 단편화 사용 X
    - 1: 단편화 사용 O
  - 세번째 비트: MF(More Fragment) 비트로, 단편화된 패킹의 마지막 패킷 여부를 나타냄.
    - 0: 뒤에 패킷이 더 있음.
    - 1: 마지막 패킷임.
- 단편화 오프셋(Fragment Offsets): 단편화 되기 전 원본 패킷 데이터에서 떨어진 거리를 나타냄
- TTL(Time To Live): 패킷의 수명을 나타냄
  - 라우터를 한번 거칠 떄마다 1씩 감소하며, 0이 될 경우 패킷은 폐기 됌.
- Protocol: 상위 프로토콜 번호를 나타냄.
- 송신지 IP 주소(Source Ip Address)와 수신지 IP 주소(Destination Ip Address): 송수신 위치 IP 주소를 나타냄

