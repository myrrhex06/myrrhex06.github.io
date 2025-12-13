---
title: "Network - 프로토콜이란?"
date: 2025-12-14 08:48:00 +0900
categories: [CS, Network]
tags: [network, protocol]
---

네트워크에서 말하는 프로토콜이란 노드들(호스트, 네트워크 장비) 간에 정보를 송수신할 때 서로 합의된 규칙을 의미함.

> 흔히 사용되는 HTTP부터 FTP, SMTP, IP, ARP 등이 모두 프로토콜임.
{: .prompt-tip }

만약 통신을 할 때 정해진 규칙(프로토콜)이 없다면, 노드들은 서로 받은 정보를 해석하지 못함.

![image](/assets/img/protocolunuse.png)

위 그림처럼 Node 1이 사용하는 한국어를 네트워크 장비가 알지 못하면 전달된 패킷을 해석할 수 없으며, 만약 Node 2까지 전달이 되더라도 Node 2는 한국어를 알지 못하기 때문에 해석이 불가능해짐.

![image](/assets/img/protocoluse.png)

정해진 규칙(프로토콜)이 있다면 위 그림처럼 네트워크 장비가 패킷의 도착지 정보를 해석 가능하며, 각 노드 간에 통신이 가능함.
