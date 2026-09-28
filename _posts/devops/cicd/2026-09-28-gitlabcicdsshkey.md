---
title: "CI/CD - Gitlab CI/CD SSH Key 등록"
date: 2025-09-28 20:50:00 +0900
categories: [DevOps, CI/CD]
tags: [devops, gitlab, ci, cd, ssh, auth]
---

## **1. 서버에 ssh 키 등록**

### **1. 내 로컬 PC에서 ssh 키 생성**

```bash
ssh-keygen -t rsa -b 4096 -f [키파일명]
```

> 파일 이름엔 생성되는 ssh key 이름 지정
{: .prompt-tip }

### **2. 공개키 내용 확인**

```bash
$ cat ./공개키명.pub
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQDPlc2A7p2LFfkgg6F...
```

출력된 공개키 내용 복사

### **3. 서버에 공개키 등록**

1.`~/.ssh` 디렉토리 접근

```bash
cd ~/.ssh
```

2.`authorized_keys` 파일에 복사해둔 공개키 등록

```bash
echo "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQDPlc2A7p2LFfkgg6F..." >> authorized_keys
```

## **2. Gitlab Runner 비밀키 등록**

### **1. 로컬 PC에서 발급받은 비밀키 내용 전체 복사**

```bash
cat ./비밀키
-----BEGIN OPENSSH PRIVATE KEY-----
b3BlbnNzaC1rZXktdjEAAAAABG5vbmUAAAAEbm9uZQAAAAAAAAABAAACFwAAAAdzc2gtcn
...
-----END OPENSSH PRIVATE KEY-----
```

출력된 내용을 모두 복사

### **2. Gitlab Repository에 비밀키 변수 설정**

CI/CD를 적용시킬 프로젝트 → setting → CI/CD → Variables → Add variable 접근하여 복사해둔 비밀키 내용을 value 값으로 한 전역 변수 추가

## **3. SSH Host Key 등록**

### **1. 로컬 PC에서 서버측 공개키 추출**

```bash
$ ssh-keyscan -p <SERVER_PORT> <SERVER_IP>
....
```

출력된 키 값들중 하나 복사

### **2. Gitlab 전역 변수 설정**

CI/CD를 적용시킬 프로젝트 → setting → CI/CD → Variables → Add variable 접근하여 복사한 서버측 공개키 전역 변수 설정