---
title: Docker - Docker Registry 구축에 대하여 알아보자.
date: 2025-07-22 21:30:00 +0900
categories: [DevOps, Docker]
tags: [devops, docker, centos]
---

서버에서 `docker pull` 커맨드로 Docker Hub에 접근이 불가능하지만, 로컬 PC에서 서버로 scp 전송이 가능한 환경(반폐쇄망)을 전제로 진행한다.

## **Docker Registry란?**
Docker Registry란 Docker Image를 저장하는 저장소로, 가장 대표적인 Docker Registry로는 Docker Hub가 존재한다.

빌드한 이미지를 업로드하고, 업로드되어 있는 이미지를 가져오는 것이 가능하다. 또한, 태그를 통해 버전 관리가 가능하다는 장점이 있다.

## **Docker Registry 구축**
1.로컬에서 Docker Registry 이미지 pull
```bash
docker pull registry:2
```

2.pull한 이미지 tar 파일로 저장
```bash
docker save -o docker_registry.tar registry:2
```

3.scp 전송
```bash
scp -P <포트번호> docker_registry.tar <계정>@<IP주소>:/<파일 전송 경로>
```

4.서버에 접속 후 이미지 load
```bash
docker load -i docker_registry.tar
```

5.load한 이미지 구동
```bash
docker run -d -p 5000:5000 -v <서버 내 마운트 경로>:/var/lib/registry registry:2
```

> 사설 Registry를 운영하려면 서버 방화벽에서 5000 포트가 열려 있어야 하며, 외부 클라이언트가 접근 가능해야 한다.
{: .prompt-tip }


### **Registry에 이미지 push**

1.이미지 태그 설정
```bash
docker tag <기존이미지명:태그> <registry주소:포트번호/이미지명:태그>
```

ex) `docker tag myimage:latest 127.0.0.1:5000/myimage:latest`

> 로컬에 있는 이미지를 공개 Registry(Docker Hub 등)가 아닌 사설 Registry에 업로드할 경우 반드시 이미지에 registry 주소가 포함되어야만 한다.
{: .prompt-tip }

2.이미지 push
```bash
docker push <registry주소:포트번호/이미지명:태그>
```

3.업로드된 이미지 확인
```bash
curl -X GET http://<registry주소:포트번호>/v2/_catalog
```

4.업로드된 이미지 태그 목록 확인
```bash
curl -X GET http://<registry주소:포트번호>/v2/<이미지명>/tags/list
```

ex) `curl -X GET http://127.0.0.1:5000/v2/myimage/tags/list`