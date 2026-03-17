---
title: "Troubleshooting - pull failed to register layer: Error processing tar file(exit status 1): archive/tar: invalid tar header 해결"
date: 2026-03-18 06:37:00 +0900
categories: [Troubleshooting]
tags: [docker, centos]
---

## **에러**
CentOS 7 환경에서 Docker 이미지를 pull 받던 도중 아래와 같은 에러가 발생함.

```bash
pull failed to register layer: Error processing tar file(exit status 1): archive/tar: invalid tar header
```

## **원인 & 해결**
Docker 버전을 확인해본 결과 20 버전을 사용하고 있었음.(2년 전 버전으로 최근 버전은 26임)

Docker 버전을 업데이트하고 다시 시도해본 결과 해결되었음.