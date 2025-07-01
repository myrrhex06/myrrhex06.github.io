---
title: Docker 이미지 빌드 및 사용 방법에 대해 알아보자.
date: 2025-07-02 06:00:00 +0900
categories: [DevOps, Docker]
tags: [devops, docker]
---

## **Docker 이미지 빌드**

Docker 이미지를 빌드하기 위해선 `Dockerfile`이 필요하다. (해당 파일이 존재한다고 가정하고 진행한다.)

> Docker 이미지란 Docker 컨테이너 실행을 위한 설계도 정도로 이해하면 된다.(추후 자세히 다룰 예정)
{: .prompt-tip }

1. Docker 이미지 빌드

```bash
docker build -t 태그명 .
```

- `docker build`: 현재 디렉터리(`.`)에 존재하는 Dockerfile을 기준으로 Docker 이미지 빌드
- `-t` 태그명: 빌드한 이미지에 이름(`tag`)를 붙임

2. Docker 이미지 tar 파일로 묶기

```bash
docker save 태그명 -o 파일명.tar
```

- `docker save`: Docker 이미지를 파일로 저장하는 명령어
- `-o 파일명.tar`: 결과물 파일 이름 지정

## **Docker 이미지 로드**

tar 파일을 통해 빌드된 Docker 이미지 로드

```bash
docker load -i tar파일경로
```

- `docker load` : 저장된 도커 이미지 파일을 도커 엔진에 불러오는 명령
- `-i` : 뒤에 이미지 파일 경로 지정

## **Docker 이미지 구동 및 종료**

Docker 이미지 컨테이너 형태로 구동

```bash
docker run -d -p 호스트포트번호:컨테이너포트번호 이미지명
```

- `docker run`: Docker 이미지 컨테이너 형태로 구동
- `-d`: 백그라운드 실행
- `-p` 호스트포트번호:컨테이너포트번호: 외부에서 접근 가능한 포트 설정

예시
```bash
docker run -d -p 8080:80 이미지명
```
위 처럼 포트를 지정할 경우 `localhost:8080`으로 접속 시 컨테이너의 80번 포트로 연결된다.

실행중인 Docker 컨테이너 확인

```bash
docker ps
```

Docker 컨테이너 종료

```bash
docker stop 컨테이너ID
```
