---
title: Java - JVM Thread Dump 개념 및 분석 방법
date: 2026-09-11 18:10:00 +0900
categories: [Language, Java]
tags: [java, jvm, thread, dump]
---

## **JVM Thread Dump란?**

- JVM 내 떠있는 Thread Snapshot을 찍는 개념
- JVM 내 모든 Thread가 어디서 뭐하고 있는지 확인 가능
- 보통 서버 성능이 저하되거나 멈췄을 때 유용함
    - ex) API 응답 성능 저하, CPU 사용량이 비정상적으로 높아짐, DB Connection 부족 등

## **Thread Dump 절차**

Java 프로세스 확인

```bash
ps -ef | grep java
```

Thread Dump 

```bash
jcmd <PID> Thread.print > <DUMP_FILE_PATH>
```

## **결과 분석**

Thread Dump를 떴을 때 Thread의 구조는 아래와 같음.

```bash
"ThreadName" #1 daemon ...
   java.lang.Thread.State: Thread.State
        StackTrace...
```

- ThreadName: 동작중인 Thread 이름
- Thread.State: Thread의 상태
- StackTrace: 해당 Thread가 현재 어떤 메서드 호출 흐름에 있는지

Thread.State 유형

| State           | 의미                        |
| --------------- | --------------------------- |
| `RUNNABLE`      | 실행 중이거나 native I/O 중 |
| `BLOCKED`       | synchronized lock 기다림    |
| `WAITING`       | 무기한 대기                 |
| `TIMED_WAITING` | 시간 제한 대기              |

예시

```bash
"boundedElastic-1" #59 daemon ...
   java.lang.Thread.State: WAITING (parking)
        at jdk.internal.misc.Unsafe.park(...)
        at java.util.concurrent.locks.LockSupport.park(...)
        ...
        at java.util.concurrent.ThreadPoolExecutor.getTask(...)
```

위 Thread는 `WAITING` 상태이지만, `ThreadPoolExecutor.getTask()`에서 새로운 작업을 기다리고 있는 상태이므로 반드시 문제가 있다고 볼 수는 없음.

> Thread Dump를 분석할 때는 단순히 State만 보는 것이 아니라 Thread 이름, Stack Trace도 함께 분석해야함.
{: .prompt-tip }