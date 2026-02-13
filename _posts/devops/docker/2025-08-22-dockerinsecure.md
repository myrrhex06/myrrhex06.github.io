---
title: Docker - Docker Insecure 설정 방법에 대해 알아보자.
date: 2025-08-22 21:00:00 +0900
categories: [DevOps, Docker]
tags: [devops, docker, linux]
---

Docker는 기본적으로 Image를 Registry에서 pull 또는 push할 때 TLS/HTTPS를 요구한다.

만약 HTTPS가 적용되어 있지 않은 Registry에서 이미지를 pull 또는 push할 경우 오류가 발생하게 된다.

이런 문제를 해결할 수 있는 방법으로 Insecure를 설정하는 방법이 있다.

## **Docker Insecure 설정**
1. daemon.json 파일 접근
```bash
vi /etc/docker/daemon.json
```

2. 아래 내용 추가
```json
{
	"insecure-registries" : ["YOUR_REGISTRY_ADDRESS"]
}
```

3. Docker 재기동
```bash
systemctl restart docker
```

이렇게, Insecure를 설정하여 HTTPS가 적용되지 않은 Registry에서도 이미지를 pull 또는 push 할 수 있다.

> 단, 운영 환경에서는 반드시 TLS/SSL을 적용해야하며, 위 방법은 테스트 환경에서만 사용하도록 하자.