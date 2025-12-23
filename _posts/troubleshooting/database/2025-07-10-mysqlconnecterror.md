---
title: MySQL 8.0 버전 Connect 오류 해결
date: 2025-07-10 20:50:00 +0900
categories: [Troubleshooting, Database]
tags: [docker, mysql]
---

## **에러 상황**
docker compose를 사용하여 Spring boot 프로젝트에서 사용할 MySQL 컨테이너를 띄우고 DBeaver를 사용하여 커넥션을 맺으려고 했으나 아래와 같은 오류가 발생했다.

![mysqlConnectionError](/assets/img/mysql_connection_error.png)

## **원인**
MySQL 8 버전부터 기본 인증 방식이 `caching_sha2_password`로 변경되었다.

> `caching_sha2_password`란 보안을 강화하기 위해 비밀번호를 해시로 처리하고, 인증 과정에서 RSA 공개키를 통해 암호화된 비밀번호를 전송한다.

따라서 클라이언트가 공개키를 요청해야하는데, 이때 MySQL 연결 URL에 특정 파라미터가 있어야 요청할 수 있다.

> 파라미터 값이 없을 경우 공개키를 받지 못하고 인증에 실패하게된다.

## **해결**
jdbc URL에 `allowPublicKeyRetrieval=true` 파라미터 추가

```
jdbc:mysql://localhost:포트명/DB이름?allowPublicKeyRetrieval=true
```

파라미터 추가 후 정상 연결되었다.

![connectionSuccess](/assets/img/mysql_connection_success.png)