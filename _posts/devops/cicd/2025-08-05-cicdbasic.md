---
title: "CI/CD에 기본 개념에 대해서 알아보자."
date: 2025-08-05 20:40:00 +0900
categories: [DevOps, CI/CD]
tags: [devops, continuous_integration, continuous_delivery, continuous_deployment]
---

개발자들은 보통 개발 업무를 진행할 때 개인이 아닌 다수(팀 단위)로 개발을 진행한다.

이때, 정해진 프로세스 없이 누군가 변경 사항을 git과 같은 형상 관리 도구에 commit하고 테스팅 없이 배포를 하게되면 문제가 발생할 확률이 매우 높다.

이런 문제를 해결하기 위해 CI(Continuous Integration)/CD(Continuous Delivery / Deployment)가 등장하였다.

## **CI/CD**?
CI/CD란 각각 Continuous Integration, Continuous Delivery/Deployment의 약자로 지속적인 통합, 배포라는 뜻을 가진다.

조금 더 자세히 살펴보면 각각 아래와 같은 역할을 한다.

- **CI (지속적 통합)**: 개발자가 push한 코드를 자동으로 빌드하고 테스트해 팀 브랜치에 병합
- **CD - Delivery (지속적 전달)**: 테스트 완료된 코드를 운영 직전 단계(릴리즈)까지 자동 배포
- **CD - Deployment (지속적 배포)**: 릴리즈된 코드를 운영 서버에 자동으로 배포

이때, Delivery와 Deployment에 자세한 차이점은 아래와 같다.

| 구분           | 설명                                       |
| -------------- | ------------------------------------------ |
| **Delivery**   | 변경사항을 릴리즈 브랜치까지 자동으로 반영 |
| **Deployment** | 릴리즈된 코드를 운영 서버에 자동 배포      |

즉, **Delivery는 배포 전까지**, **Deployment는 실제 배포까지** 자동화된다는 차이다.

### **CI/CD의 장점**
- 테스팅을 선택이 아닌 강제화 할 수 있음
- 자동화된 파이프라인 구성 가능
  - 빌드, 테스팅, 배포 프로세스가 명확히 잡혀있어 개개인별로 처리하는게 아닌 모두가 자동화된 동일한 프로세스에 맞춰 통합 및 배포가 이뤄질 수 있음

> 파이프라인이란 코드 작성부터 배포까지의 모든 과정들을 말한다.

CI/CD는 일반적으로 아래와 같은 프로세스로 동작하게 된다.

```
[개발자 PUSH]
-> [CI: Build -> Test -> Merge]
-> [CD: Delivery -> Deployment]
```

### **Build**
개발자가 작성한 소스 코드만으로는 서비스를 바로 운영할 수는 없다.<br>
이때, Build는 소스 코드를 실행 가능한 형태로 패키징 하는 과정들을 말한다.

**대표적인 Build Tool**
- Maven, Gradle: Java 및 Android에서 주로 사용하는 빌드 도구
- Webpack: JavaScript 기반 프론트엔드 모듈 번들러

### **Test**
작성한 코드가 문제없이 잘 동작하는지 검증하는 단계이다.

대표적인 테스팅 프레임워크로는 Junit, Jest 등이 존재한다.

**테스트 유형**
- **단위(Unit) 테스트**: 함수나 클래스 등 작은 단위 테스트
- **통합(Integration) 테스트**: 여러 모듈을 조합해 테스트
- **E2E(End-to-End) 테스트**: 실제 사용 환경을 시뮬레이션

### **Merge**
변경 사항을 하나의 브랜치(보통은 develop)에 병합하는 것을 뜻한다.<br>
**Git**을 통해 병합하며, **충돌(Conflict)** 해결이 핵심이다.

> CI는 보통 Merge 전에 자동으로 테스트를 돌려 문제가 없는지 검증한다.

### **Deployment**
개발한 결과물을 실제 운영 서버에 반영하는 단계이다.<br>
CI/CD 파이프라인을 통해 배포 단계까지 자동화가 가능하다.