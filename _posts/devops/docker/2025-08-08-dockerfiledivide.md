---
title: Docker - Dockerfile 분리에 대해서 알아보자.
date: 2025-08-08 22:00:00 +0900
categories: [DevOps, Docker]
tags: [devops, docker, dockefile]
---

실무에서 개발을 진행할 때 일반적으로 설정 파일을 `local`, `dev` 등으로 나눠서 환경에 따라 관리한다.

docker image를 빌드시킬 때 사용하는 `Dockerfile`도 이처럼 분리하여 관리가 가능하다.

## **환경별 분리**

`local.Dockerfile` 예시

```docker
FROM openjdk:17-jdk-alpine

WORKDIR /app

COPY target/example-0.0.1.jar example-0.0.1.jar

EXPOSE 9000

ENTRYPOINT ["java", "-jar", "example-0.0.1.jar"]
```

`dev.Dockerfile` 예시

```docker
FROM openjdk:17-jdk-alpine

WORKDIR /app

COPY target/example-0.0.1.jar example-0.0.1.jar

EXPOSE 11000

ENTRYPOINT ["java", "-jar", "example-0.0.1.jar"]
```

빌드 커맨드

```bash
docker build -f dev.Dockerfile -t image명 .
```

- `-f`: 사용할 `Dockerfile` 명시
  - 기본은 `Dockerfile`, 환경에 따라 `-f dev.Dockerfile`처럼 분리해서 빌드 가능

이렇게 `Dockerfile`을 환경별로 분리하면 유지보수와 배포 자동화에 유리하며, 불필요한 수정 없이 효율적인 이미지를 생성할 수 있다.