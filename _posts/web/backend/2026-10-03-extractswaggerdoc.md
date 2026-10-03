---
title: "Backend - Swagger API Docs 추출"
date: 2026-10-03 19:20:00 +0900
categories: [Web, Backend]
tags: [spring, springboot, swagger, html]
---

1.Swagger ui에서 `api-docs.json` 추출
- `http://ip:port/v3/api-docs` 접속

2.swagger ui html2 추출 커맨드

```bash
docker run --rm -v YOUR_API_DOCS_JSON_DIRECTORY_PATH:/local 
openapitools/openapi-generator-cli generate 
-i /local/api-docs.json 
-g html2 
-o /local/docs --skip-validate-spec
```