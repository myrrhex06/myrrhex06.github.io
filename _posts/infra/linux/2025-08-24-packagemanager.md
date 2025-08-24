---
title: 패키지 매니저(package manager)에 대해 알아보자.
date: 2025-08-24 19:20:00 +0900
categories: [Infra, Linux]
tags: [ubuntu, apt, packagemanager]
---

## **패키지 매니저(Package Manager)란?**
패키지 매니저란 개발 환경에서 패키지(소프트웨어, 라이브러리 등)를 설치할 때 사용하는 것을 말한다.

운영 환경에 따라서 사용하는 패키지 매니저도 달라진다.
- CentOS, Rocky Linux: `yum` 또는 `dnf`
- Ubuntu: `apt`

이런 패키지 매니저는 설치 뿐만 아니라 업데이트, 제거에도 사용되기 때문에 패키지 관리 도구라고 생각하면된다.

## **패키지 매니저 명령어**

사용하는 패키지 매니저는 Ubuntu Linux를 기준으로 설명한다.

### **패키지 설치**
```bash
sudo apt install [패키지명]
```

위 명령어를 실행할 경우 패키지 저장소로부터 해당 패키지를 다운로드한다.

주의해야할 점은 apt 사용 시 `sudo`를 붙여야만한다.

붙이지 않을 경우 아래와 같은 오류가 발생하게된다.
```bash
E: Could not open lock file /var/lib/dpkg/lock-frontend - open (13: Permission denied)
E: Unable to acquire the dpkg frontend lock (/var/lib/dpkg/lock-frontend), are you root?
```

### **패키지 목록 최신화**

패키지 저장소는 개발자들에 의해 수백개의 패키지가 업데이트 된다.<br>

이 패키지 저장소는 실시간으로 동기화되지 않기 때문에, 패키지를 설치하기 전에 패키지 목록을 최신화시켜줘야한다.

```bash
sudo apt update
```

## **설치된 패키지 확인**
```bash
sudo apt list --installed
```

설치된 패키지는 위 명령어를 실행시켜 확인이 가능하다.

```bash
sudo apt list --installed | grep [패키지명]
```

또한, `apt install`로 설치한 패키지가 잘 설치됬는지 확인하기 위해서는 위 명령어 처럼 `grep`을 사용하여 설치를 진행한 패키지만 확인하는 것도 가능하다.

### **설치된 패키지 삭제**
```bash
sudo apt purge --auto-remove [패키지명]
```

설치된 패키지와 관련 파일을 모두 제거하기 위해선 위 명령어를 사용하면 된다.

> `apt remove` 명령어도 있지만, 이 명령어는 관련 파일까지 제거해주진 않기 때문에 깔끔한 제거를 원한다면 `apt purge` 사용이 유용하다.