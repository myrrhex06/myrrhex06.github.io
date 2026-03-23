---
title: "Backend - 대용량 파일 생성 커맨드"
date: 2026-03-24 06:35:00 +0900
categories: [Web, Backend]
tags: [file]
---

대용량 파일 업로드 테스트를 위한 파일 생성 커멘드

5GB 파일 생성 커맨드
```bash
fsutil file createnew test_5GB.bin 5368709120
```