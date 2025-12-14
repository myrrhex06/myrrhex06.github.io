---
title: "Network - 트래픽과 네트워크 성능 지표란?"
date: 2025-12-14 12:26:00 +0900
categories: [CS, Network]
tags: [network, trafic, throughput, bandwidth, packetloss]
---

## **트래픽이란?**

누구나 한번씩 들어봤을 용어임.

트래픽이란 네트워크 내의 정보량을 의미함.

특정 노드에 트래픽이 몰린다는 것은 해당 노드가 특정 시간 동안 처리해야할 정보량이 많다는 것을 의미함.

특정 노드에 트래픽이 몰리게되면 과부화 현상이 나타날 수 있고, 해당 현상이 성능 저하까지 이어질 수 있음.

## **네트워크 성능 지표란?**

네트워크의 성능을 확인할 수 있는 지표들을 의미함.

해당 지표들을 통해 네트워크의 과부화 현상이 나타났는지, 처리 성능은 어떤지 확인할 수 있음.

보통 네트워크 성능 지표는 아래 단위를 통해 나타냄
- bps(bit/s): 초당 bit 수
- Mbps(Mbit/s): 초당 Mbit 수
- Gbps(Gbit/s): 초당 Gbit 수
- pps(p/s): 초당 packet 수

### **처리율(throughput)**

단위 시간 동안 네트워크를 통해 실제로 전송되는 정보량을 의미함.

비교적 실시간성이 강조되는 지표로, 특정 노드가 얼마만큼의 트래픽을 처리하는 중인지 확인할 때 주로 사용됨.

### **대역폭(bandwidth)**

단위 시간 동안 통신 매체를 통해 송수신될 수 있는 최대 정보량을 의미함.

### **패킷 손실(packet loss)**

용어 그대로 송수신 과정 도중 패킷이 손실되는 상황을 의미함.

순간적으로 트래픽이 몰리거나, 네트워크 장애 등으로 발생할 수 있음.

`ping` 커맨드를 통해 패킷이 손실되었는지 확인이 가능함.

```bash
ping -c 5 8.8.8.8

PING 8.8.8.8 (8.8.8.8): 56 data bytes
64 bytes from 8.8.8.8: icmp_seq=0 ttl=116 time=36.927 ms
...

--- 8.8.8.8 ping statistics ---
5 packets transmitted, 5 packets received, 0.0% packet loss
round-trip min/avg/max/stddev = 36.340/36.912/38.305/0.725 ms
```

결과를 확인해보면 

```bash
5 packets transmitted, 5 packets received, 0.0% packet loss
```

패킷이 모두 전송되었고, 손실된 패킷은 0%라고 나옴.