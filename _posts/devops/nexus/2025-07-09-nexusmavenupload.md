---
title: Nexus - Nexus Repository Maven 연동 방법에 대해 알아보자.
date: 2025-07-09 20:00:00 +0900
categories: [DevOps, Nexus]
tags: [nexus, docker, maven, spring, java]
---

Nexus Repository에 생성한 Maven Repository에 빌드 결과물(Jar, war 등)을 올리는 방법에 대해 알아보자.

> 이 글에서는 Spring boot 프로젝트를 빌드한 jar 파일을 업로드하는 과정을 정리한다.

## **Maven Repository 연동**
1.jar 또는 war 파일 빌드
```bash
mvn -DskipTests clean package
```
- `-DskipTests`: 테스트 절차를 무시하는 옵션

2.생성한 Nexus Repository URL 확인

Setting 메뉴 > Repository > 생성한 Repository를 클릭하면 확인 가능하다.

![url](/assets/img/nexus_maven_repository_url.png)

3.`pom.xml`에 아래 내용 추가
```xml
<distributionManagement>
	<repository>
		<id>nexus-maven</id>
		<url>http://localhost:8081/repository/maven_test</url>
	</repository>
</distributionManagement>
```
- `distributionManagement`: 배포 설정 태그(`mvn deploy` 할 때만 사용됨)
- `repository`: Release 버전의 결과물을 배포할 Nexus 저장소 경로 설정
  - `snapshot`은 `snapshotRepository` 태그를 사용하여 따로 처리해줘야함
- `id`: `setting.xml`에 인증 정보를 넣을 때 사용되기에 매우 중요.
  - `id` 값은 정해진게 없으나 중복값이 있으면 안됨.
- `url`: 2번에서 확인한 URL 정보 입력

> `setting.xml`은 Maven에서 사용하는 개인 설정 파일로, 주로 인증 정보, 기본 저장소 설정 등의 정보가 들어있다.
{: .prompt-tip }

4.`~/.m2` 경로에 접속하여 `setting.xml` 수정

```bash
vim setting.xml
```

`setting.xml` 파일
```xml
<servers>
  <server>
    <id>pom.xml에서 작성한 id</id>
    <username>Nexus 계정 Id</username>
    <password>Nexus 계정 password</password>
  </server>
</servers>
```

5.maven 배포 커맨드 실행
```bash
mvn deploy
```

6.Nexus Repository에서 결과 확인

![result](/assets/img/nexus_maven_result.png)

## **Troubleshooting**

**발생 에러**

![error](/assets/img/nexus_maven_error.png)

**Deployment policy**가 **Disable redeploy**로 설정되어 있어서 발생한 에러.

**해결**

![resolve](/assets/img/nexus_maven_resolve.png)

**Allow redeploy**로 재설정하여 해결
