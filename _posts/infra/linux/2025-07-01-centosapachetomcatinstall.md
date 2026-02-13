---
title: Linux - CentOS 7 Apache Tomcat 설치 및 구성 방법에 대해 알아보자.
date: 2025-07-01 06:00:00 +0900
categories: [Infra, Linux]
tags: [centos, apache, tomcat]
---

지원이 종료되긴 했지만 여전히 실무에서 많이 사용하는 리눅스 배포판 중 하나인 CentOS 7 환경에서 Apache Tomcat 설치 및 구성하는 방법에 대해 알아보자.

## Apache Tomcat 설치 및 구동
Apache Tomcat을 설치하기 위해서 아래 절차대로 진행한다.

1. Apache Tomcat에 tar 파일 다운로드(해당 포스팅에서는 9.0.104 버전을 기준으로 설명)
```bash
wget https://archive.apache.org/dist/tomcat/tomcat-9/v9.0.104/bin/apache-tomcat-9.0.104.tar.gz
```

> `wget` 커맨드는 웹 서버에서 파일을 다운로드할 때 사용된다.
{: .prompt-tip }

2. 다운로드 받은 tar 파일 압축 풀기
```bash
tar -xvzf apache-tomcat-9.0.104.tar.gz
```

`tar` 커맨드 각 옵션 설명
- -x: 압축 풀기
- -v: 어떤 파일이 풀리는지 출력
- -z: .gz 확장자의 파일을 처리할 때 필요
- -f: 뒤에 오는 파일 이름이 압축 대상임을 명시

3. 실행 권한 설정
```bash
cd apache-tomcat-9.0.104
chmod +x bin/*.sh
```

4. Tomcat 포트 설정(Option)

`server.xml` 저장 경로로 이동
```bash
cd apache-tomcat-9.0.104/conf
vim server.xml
```

포트 설정 진행
```xml
 <!-- port 수정 -->
 <Connector port="8080" protocol="HTTP/1.1" 
               connectionTimeout="20000"
               redirectPort="8443"
               maxParameterCount="1000"
/> 
```

5. Tomcat 구동
```bash
apache-tomcat-9.0.104/bin/startup.sh
```

6. Tomcat 종료
```bash
apache-tomcat-9.0.104/bin/shutdown.sh
```

## .war 파일을 배포할 경우 webapp 디렉토리에 넣는 war 파일의 이름을 ROOT.war로 설정하는 이유
기본 URL(/)로 바로 실행되게끔 하기 위해서이다.

Tomcat은 war 파일명을 기준으로 URL 경로를 자동으로 매핑한다.

ex) `example.war`일 경우 `http://localhost:8080/example/` 로 잡히게 된다.<br>
`ROOT.war`로 할 경우 `http://localhost:8080/` 으로 바로 접속할 수 있게 된다.

## server.xml 파일에서 Port 설정은 Connector 태그에 port 속성으로 하는데 Server 태그에 port 설정은 뭘까?
- `Connector` 태그 port 속성: HTTP 서비스 포트
  - 사용자가 웹 브라우저로 접근하는 포트
- `Server` 태그 port 속성: Tomcat 종료 포트
  - 내부적으로 shutdown signal 보낼 때 사용
  - 보안상의 이유로 localhost에서만 동작하며, `shutdown.sh`를 실행시킬 때만 사용
