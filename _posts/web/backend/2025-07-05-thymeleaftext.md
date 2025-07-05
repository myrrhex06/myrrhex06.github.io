---
title: Thymeleaf - text, utext
date: 2025-07-06 01:00:00 +0900
categories: [Web, Backend]
tags: [backend, ssr, thymeleaf, spring]
---

Thymeleaf에서 `text`를 출력하는 기능에 대해 알아보자.

들어가기에 앞서 타임리프를 사용하기 위해선 아래 코드처럼 `html` 태그에 `xmlns` 속성으로 연결해줘야한다.

```html
<html xmlns:th="http://www.thymeleaf.org">
```

## **`text`와 `utext`**
`Model`로 넘긴 데이터를 타임리프에서 `text` 형태로 렌더링 하기 위해서는 `th:text` 속성을 사용한다.

**예시**<br>
`ExampleController.java`
```java
@GetMapping("/text")
public String text(Model model) {
  model.addAttribute("text", "Hello World!");
        
  return "example/text";
}
```

`example/text.html`
```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h1>Thymeleaf - Text</h1>
<p th:text="${text}"></p>
<p>[[${text}]]</p>
</body>
</html>
```

> `[[${...}]]` 표현식을 사용하여 `th:text` 속성을 지정하지 않고 바로 렌더링하는 것도 가능하다.
{: .prompt-tip }

결과<br>
![thtextresult](/assets/img/thtextresult1.png)

이런 `th:text` 또는 `[[${...}]]` 표현식을 **사용할 때 주의점은 HTML 엔티티로 변경되는 이스케이프**이다.

> 이스케이프란 HTML에서 사용되는 특수문자(`<`, `>` 등)를 HTML 엔티티(`&lt;`, `&gt;`)로 변경하는 것을 말한다.
{: .prompt-tip }

이스케이프 기능을 사용하지 않게 하기위해선 `Unescape` 처리를 해줘야한다.<br>
이때 사용하는 속성이 `th:utext` 속성이다.

**예시**<br>
`ExampleController.java`
```java
@GetMapping("/text")
public String text(Model model) {
    ...
    model.addAttribute("untext", "<b>Hello World!</b>");

  return "example/text";
}
```

`example/text.html`
```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

<h1>Thymeleaf - Unescape</h1>
<p th:utext="${untext}"></p>
<p>[(${untext})]</p>
</body>
</html>
```

결과<br>
![untext](/assets/img/thutextresult1.png)

> `[(...)]` 표현식을 사용하여 `th:utext` 속성을 사용하지 않고 `Unescape` 처리가 가능하다.
{: .prompt-tip }

만약 `th:utext` 속성을 사용하지 않고 `th:text` 속성만을 사용하여 처리했다면 위 코드에서는 <b> 태그가 제대로 적용되지 않고 HTML 엔티티로 적용되어 태그 그대로가 출력되었을 것이다.
