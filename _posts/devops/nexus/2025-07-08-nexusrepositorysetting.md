---
title: Nexus Repository에 대해 알아보자.
date: 2025-07-08 18:00:00 +0900
categories: [DevOps, Nexus]
tags: [nexus, docker, nexus]
---

## **Nexus Repository란?**
Nexus Repository란 사설 아티팩트 저장소로 `Maven`, `Docker`, `Npm` 등 여러 패키지 저장소 관리가 가능하다.<br>

주로 사내 또는 폐쇄망 환경에서 라이브러리, 빌드 결과물(jar, war 등)을 관리하는데 사용한다.

## **Nexus Repository 구성**
- `Hosted`: 직접 아티팩트를 업로드해서 사용하는 저장소
- `Proxy`: 외부 중앙 저장소(Maven Central, Npm Registry 등) 캐싱
- `Group`: 여러 저장소를 묶어서 단일 URL로 접근 가능하게 함

## **Nexus Repository 구동**
docker image를 통해 쉽게 구동이 가능하다.

1.Docker run 명령어 실행

```bash
docker run -d -p 8081:8081 --name nexus sonatype/nexus3
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

!['settings'](/assets/img/nexus_settings.png)

> Blob Store란 실제 파일이 물리적으로 저장되는 공간을 뜻한다.