---
title: "Docker - 구동중인 컨테이너 정보 백업하기"
date: 2026-03-17 20:17:00 +0900
categories: [DevOps, Docker]
tags: [devops, docker, linux]
---

사용 커맨드
```bash
docker inspect $(docker ps -q) > docker_full_backup.json
```