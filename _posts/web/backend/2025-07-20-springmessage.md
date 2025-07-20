---
title: Spring boot 메시지, 국제화에 대해 알아보자.
date: 2025-07-20 09:00:00 +0900
categories: [Web, Backend]
tags: [backend, spring, message, i18n]
---

## **메시지란?**

메시지란 Spring boot에서 기본적으로 제공하는 기능 중 하나로, 웹 애플리케이션에서 사용되는 문자들을 `messages.properties` 파일에 정의해두고 사용하는 것을 말한다.

 이 방식의 장점으로는 웹 애플리케이션에서 사용되는 문자들을 일일이 수정하지 않고 `messages.properties` 파일만 수정하면 된다는 것이다.

## **국제화란?**

국제화란 앞서 설명한 메시지 기능을 활용해서 사용자 브라우저 언어 설정에 따라 언어를 변경하는 것을 말한다.

`Accept-Language` 헤더를 통해 브라우저가 설정한 언어 정보를 확인한다.


국제화도 `messages.properties` 파일을 사용하는데, 사용 방식은 `messages_ko.properties`, `messages_en.properties` 이런 식으로 사용하게 된다.

## **Spring boot 메시지, 국제화**
이제 Spring boot에서 메시지, 국제화 설정 및 사용 방법에 대해 알아보자.

### **메시지, 국제화 설정**

Spring boot에서는 아래와 같이 기본적으로 `MessageSource`라는 인터페이스를 `Bean`으로 등록해준다.

```java
@Bean
public MessageSource messageSource() {
  ResourceBundleMessageSource messageSource = new ResourceBundleMessageSource();
  messageSource.setBasenames("messages");
  messageSource.setDefaultEncoding("UTF-8");
  return messageSource;
}
```

`basename`으로 등록한 이름을 기준으로 `properties` 파일을 읽는다.<br>
위 코드를 기준으로 설명하자면 `messages`로 시작하는 `resources` 패키지 아래에 있는 파일들을 읽는다.

만약 `basename`을 커스터마이징 해야한다면 `application.properties`(또는 `.yml`) 파일에 아래와 같이 내용을 추가하면 된다.

```properties
spring.messages.basename=messages,i18n.messages
```

> `i18n`이란 internationalization의 줄임말로 i로 시작하여 n으로 끝나며 그 사이에 18개의 글자가 존재한다는 뜻이다.
{: .prompt-tip }

`messages.properties` 파일들을 생성할 땐 `resources` 패키지 아래에 생성한다.

`messages.properties` 예시
```properties
example=예시
example.ex=예시 {0}
```

`{0}`은 파라미터를 받겠다는 뜻으로, `Message`를 설정할 때 파라미터를 넣게 지정하는 것도 가능하다.

`messages_en.properties` 예시
```properties
example=example
example.ex=example {0}
```

위와 같이 `en`을 붙여 생성하게 되면 `Accept-Language` 헤더에 값이 `en`일 경우 자동으로 `messages_en.properties` 파일을 읽게 된다.

### **설정한 메시지, 국제화 사용**

`MessageSource`를 통해 설정한 메시지, 국제화 기능을 사용할 수 있다.

`MessageController.java`
```java
@GetMapping("/default")
public ResponseEntity<String> defaultMessage() {
  String example = messageSource.getMessage("example", null, Locale.ROOT);

  return ResponseEntity.ok(example);
}
```

`getMessage` 메서드를 통해 설정해둔 `Message`를 가져올 수 있다.
- 1번째 매개변수: `properties` 파일에 설정해둔 코드
- 2번째 매개변수: 넘겨줄 파라미터
- 3번째 매개변수: `Locale`(지역 정보) `ROOT` 또는 `null`값 전달 시 기본 `messages.properties`가 사용됨

요청 결과<br>
![result1](/assets/img/result1.png)

`Message`를 불러올 때 파라미터를 넘기는 것도 가능하다.

```java
@GetMapping("/parameter")
public ResponseEntity<String> parameterMessage() {
  String example = messageSource.getMessage("example.ex", new Object[]{"Parameter"}, Locale.ROOT);
  return ResponseEntity.ok(example);
}
```

요청 결과<br>
![result2](/assets/img/result2.png)

불러오려고 하는 `Message code`가 존재하지 않을 경우엔 `NoSuchMessageException`이 발생하게 되는데, 이를 방지하기 위해 기본값 설정도 가능하다.

```java
@GetMapping("/default-value")
public ResponseEntity<String> defaultValueMessage() {
  String example = messageSource.getMessage("notExists", null, "기본값", Locale.ROOT);
  return ResponseEntity.ok(example);
}
```

요청 결과<br>
![result3](/assets/img/result3.png)

`Locale` 정보를 설정하여 국제화 기능을 사용하는 것도 가능하다.

```java
@GetMapping("/english")
public ResponseEntity<String> englishMessage() {
  String example = messageSource.getMessage("example", null, Locale.ENGLISH);
  return ResponseEntity.ok(example);
}
```

요청 결과<br>
![result4](/assets/img/result4.png)


## **IntelliJ 한글 깨짐 해결**

1.Settings > File Encodings에 접속 
2.Default encoding for properties files 옵션을 UTF-8로 지정
3.Transparent native-to-ascii conversion 체크박스 활성화

![settings](/assets/img/encodingsettings.png)

4.Invalidate Caches 실행

![invalidateCaches](/assets/img/invalidateCaches.png)

