---
title: "App Store에 업로드된 앱 버전 업데이트 방법"
date: 2026-03-01 18:40:00 +0900
categories: [Mobile, iOS]
tags: [app, appstore, deploy, version]
---

### **1. Target 설정 version, build 버전 정보 업데이트**

아래 이미지에서 볼 수 있듯이 가장 먼저 앱의 Version, Build 버전 정보를 업데이트해야함.

- Version: 사용자에게 보이는 앱 버전
- Build: 내부 배포용 번호

![image](/assets/img/apptargetversionbuildupdate.png)

### **2. 앱 빌드 파일 Archive**

Version 정보를 업데이트 했다면, Archive를 해줌.

![image](/assets/img/appproductarchive.png)

### **3. Distribute App**

Archive가 완료되면 Distribute App을 실행함.

![image](/assets/img/appdistribute.png)

### **4. App Store Connect 페이지 접속 후 새 버전 등록**

업데이트할 앱 정보에 접속한 후 + 버튼을 통해 새 버전을 추가.

![image](/assets/img/appledevelopernewversionadd.png)

업데이트 사항을 작성하면 됨.

> 미리보기 이미지, 프로모션 텍스트를 수정할 수 있으며, 변경 사항이 없을 경우 그대로 두면 됨.
{: .prompt-tip }

## **Reference**
- [https://velog.io/@nowlee/%EC%95%B1%EC%8A%A4%ED%86%A0%EC%96%B4-%EC%95%B1-%EC%97%85%EB%8D%B0%EC%9D%B4%ED%8A%B8%ED%95%98%EB%8A%94-%EB%B0%A9%EB%B2%95](https://velog.io/@nowlee/%EC%95%B1%EC%8A%A4%ED%86%A0%EC%96%B4-%EC%95%B1-%EC%97%85%EB%8D%B0%EC%9D%B4%ED%8A%B8%ED%95%98%EB%8A%94-%EB%B0%A9%EB%B2%95)