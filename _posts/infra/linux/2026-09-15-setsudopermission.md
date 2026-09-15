---
title: "Linux - 사용자 추가, sudo 권한 설정"
date: 2026-09-15 20:03:00 +0900
categories: [Infra, Linux]
tags: [ubuntu, user, sudo, permission]
---

1. 사용자 추가

```bash
sudo useradd 사용자명
```

2. 비밀번호 설정

```bash
sudo passwd 사용자명
```

3. sudo 권한 설정

```bash
sudo usermod -aG sudo 사용자명
```

4. sudo 그룹 확인

```bash
groups 사용자명
```