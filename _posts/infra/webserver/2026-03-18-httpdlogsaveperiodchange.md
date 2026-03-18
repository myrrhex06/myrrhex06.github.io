---
title: "Web Server - Apache Web Server(httpd) 로그 보관 기간 변경 방법"
date: 2026-03-01 19:12:00 +0900
categories: [Infra, WebServer]
tags: [centos, apache, httpd, logrocate]
---

Apache Web Server 로그 보관 기간을 6개월로 늘려야하는 상황이 생겨 정리함.

### **1. 디스크 용량 체크**
먼저 6개월 동안 계속해서 쌓이는 로그 파일을 보관해야하기 때문에 충분한 용량이 있는지 확인해야함.

```bash
# 서버 전체 디스크 용량 확인
df -h

# 개별 파일(or 디렉토리) 디스크 사용 용량 확인
du -sh /log_directory_path
```

### **2. log 파일 경로 확인**

용량이 충분하다면 현재 Apache가 어디에 로그를 쌓고 있는지 확인해야함.

```bash
lsof -p $(pgrep httpd | head -1) | grep log
```

> `httpd.conf` 파일에서 직접 로그 파일이 저장되는 경로를 확인하는 것도 가능함.
{: .prompt-tip }

### **3. logrotate 설정 파일 수정**

`logrotate.d` 디렉토리에 접근 후 설정 파일을 편집해야함.
```bash
# 디렉토리 경로 확인
find / -name "logrotate.d" -type d

# logrotate 경로 접근
cd /path/logrotate.d

# httpd 파일 편집
vim httpd
```

아래와 같이 보관 기간을 설정해줌
```bash
/var/log/httpd/*log {
    daily # 하루 단위 로테이션
    missingok
    notifempty
    dateext
    sharedscripts
    rotate 180 # 보관 기간 180일 설정
    compress # gz 압축 옵션
    delaycompress
    postrotate
        /bin/systemctl reload httpd.service > /dev/null 2>/dev/null || true
    endscript
}
```

그후 마지막으로 문법 오류가 있는지 검증해야함.

```bash
logrotate -d /path/logrotate.d/httpd
```