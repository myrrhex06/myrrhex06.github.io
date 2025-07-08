---
title: Nexus Repository에 대해 알아보자.
date: 2025-07-08 18:00:00 +0900
categories: [DevOps, Nexus]
tags: [nexus, docker, nexus]
---

## **Nexus Repository란?**
Nexus Repository는 기업 내부에서 개발/배포 과정에서 필요한 패키지(jar, npm, docker 이미지 등)를 관리하는 사설 저장소다.  

외부 저장소 연결이 끊기면 배포 못 하는 상황 방지 및 보안 관리 목적으로 사용한다.

## **Nexus Repository 구성**
- `Hosted`: 직접 아티팩트를 업로드해서 사용하는 저장소
- `Proxy`: 외부 중앙 저장소(Maven Central, Npm Registry 등) 캐싱
- `Group`: 여러 저장소를 묶어서 단일 URL로 접근 가능하게 함

## **Nexus Repository 구동**
docker image를 통해 쉽게 구동이 가능하다.

1.Docker run 명령어 실행

```bash
docker run -d -p 8081:8081 --name nexus -v nexus-data:/nexus-data sonatype/nexus3
```

2.지정한 포트로 접속

![대시보드](/assets/img/nexus_dashboard.png)

> 위의 예시로는 localhost:8081로 접속

3.Nexus 관리자 계정 로그인
- id: admin

비밀번호의 경우 아래 커맨드를 실행하여 확인 가능하다.
```bash
docker exec -it <컨테이너 이름> cat /nexus-data/admin.password
```

로그인 후 기타 설정들을 진행한다.

4.Settings 메뉴로 들어오면 Repository, Blob Stores 등 구성이 가능하다.

![settings](/assets/img/nexus_settings.png)

> Blob Store란 실제 파일이 물리적으로 저장되는 공간을 뜻한다.

## **Maven Repository 생성**
1.Settings 메뉴에서 Repository 선택 후 Create Repository 클릭

![step1](/assets/img/nexus_settings_repository.png)

2.Maven2(hosted) 선택

![step2](/assets/img/nexus_settings_maven.png)

3.Blob Store 선택 후 Create Repository 클릭

![step3](/assets/img/nexus_settings_create_repository.png)

> 기본 제공(default)을 사용할지 Blob Store를 새로 만들어서 사용할지는 상황에 따라 설정