---
title: "Docker - Apache Age 구성"
date: 2026-09-20 12:25:00 +0900
categories: [DevOps, Docker]
tags: [devops, docker, apache, postgres, database]
---

편의를 위해서 docker-compose를 사용하여 컨테이너 형태로 구동시킴.

`docker-compose.yml`
```yaml
services:
  postgres-age:
    image: apache/age:latest
    container_name: postgres-age-db
    ports:
      - "5432:5432"
    environment:
      POSTGRES_USER: YOUR_USERNAME
      POSTGRES_PASSWORD: YOUR_PASSWORD
      POSTGRES_DB: YOUR_DATABASE
    volumes:
      - YOUR_VOLUMES_PATH:/var/lib/postgresql
    restart: unless-stopped
```

`psql` 접속
```bash
docker exec -it <YOUR_CONTAINER_ID> psql -U <YOUR_USERNAME>
```

아래 쿼리 실행
```sql
-- AGE 확장 로드 --
LOAD 'age'; 

-- AGE 전용 카탈로그 search_path에 추가 --
SET search_path = ag_catalog, "$user", public; 

-- 그래프 생성 --
SELECT CREATE_GRAPH('YOUR_GRAPH_NAME');
```