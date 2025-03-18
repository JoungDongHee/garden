---
date: 2025 년 03 월 18 일 13 시 03 분
tags:
  - Dev
  - Thymeleaf
  - link
  - 링크
author: Joung Dong Hee
share: true
---

# 링크 - link

Thymeleaf에서는 `@{...}` 문법을 사용하여 동적인 링크를 생성할 수 있다.

## 1. 고정된 링크 생성

고정된 링크는 다음과 같이 생성할 수 있다.

```html
<li><a th:href="@{/hello}">Basic URL</a></li>
```

이 경우 생성된 링크는 다음과 같다.

```txt
/hello

```

## 2. 쿼리 파라미터(Query Parameter) 사용

쿼리 파라미터를 추가하려면 URL 뒤에 `(key=value, key=value, ...)` 형식으로 작성하면 된다.

### 예제 코드 (Java)

```java
model.addAttribute("param1", "data1");
model.addAttribute("param2", "data2");
```

### 예제 코드 (HTML)

```html
<li><a th:href="@{/hello(param1=${param1}, param2=${param2})}">Query Parameter</a></li>
```

### 실제 생성된 링크

```txt
/hello?param1=data1&param2=data2
```

## 3. Path Variable 사용

`Path Variable`을 사용하여 동적인 경로를 구성할 수 있다. 경로 변수는 `{}` 내부에 정의하며, `( )` 안에서 해당 값과 매핑한다.

### 예제 코드 (HTML)

```html
<li><a th:href="@{/hello/{param1}/{param2}(param1=${param1}, param2=${param2})}">Path Variable</a></li>

```

### 실제 생성된 링크

```txt
/hello/data1/data2
```

## 4. Path Variable + Query Parameter 조합

Path Variable과 Query Parameter를 함께 사용할 수도 있다.

```html
<li><a th:href="@{/hello/{param1}(param1=${param1}, param2=${param2})}">Path Variable + Query Parameter</a></li>
```

### 실제 생성된 링크

```txt
/hello/data1?param2=data2
```


> [!info] info
> `param1`은 `{param1}`에 매핑되었고, `param2`는 쿼리 파라미터로 자동 변환된다.

## 5. 상대 경로 및 절대 경로

- **상대 경로**: 현재 URL을 기준으로 이동
    - 현재 URL이 `/page`라면 생성된 링크는 `/page/hello`가 된다.

```html
<a th:href="@{hello}">Relative Link</a>
```

- **절대 경로**: 루트(`/`) 기준으로 이동
    - 항상 `/hello`로 이동

```html
<a th:href="@{/hello}">Absolute Link</a>
```

## 6. URL에 컨텍스트 패스를 포함하는 경우

Thymeleaf는 자동으로 컨텍스트 패스를 포함한다. 따라서 `@{}` 내부에서 컨텍스트 패스를 직접 추가할 필요가 없다.

예를 들어, `@{/hello}`는 `http://localhost:8080/hello`로 변환되며, 컨텍스트 패스가 `/myapp`이라면 `http://localhost:8080/myapp/hello`로 변환된다.

만약 컨텍스트 패스를 직접 추가하려면 `${#httpServletRequest.contextPath}`를 사용할 수도 있다.

```html
<li><a th:href="@{${#httpServletRequest.contextPath} + '/hello'}">Context Path Manual</a></li>
```