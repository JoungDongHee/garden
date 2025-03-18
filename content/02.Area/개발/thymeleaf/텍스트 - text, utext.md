---
date: 2025 년 03 월 18 일 11 시 03 분
tags:
  - Dev
  - text
  - utext
  - Thymeleaf
author: Joung Dong Hee
share: true
---
# 텍스트 출력 - `th:text`와 `th:utext`

## `th:text`

`th:text`는 Thymeleaf에서 가장 기본적인 기능으로, 단순한 텍스트를 출력하는 데 사용됩니다.

### 사용 예시

```html
<ul>  
    <li>th:text 사용: <span th:text="${data}"></span></li>  
    <li>컨텐츠 안에서 직접 출력하기: [[${data}]]</li>  
</ul>

```

위와 같이 선언하면, 실제 화면에서는 `${data}`의 값이 치환되어 출력됩니다.

```html
<ul>
    <li>th:text 사용: <span>Hello Spring!</span></li>
    <li>컨텐츠 안에서 직접 출력하기: Hello Spring!</li>
</ul>
```

Thymeleaf에서 데이터를 있는 그대로 출력하려면 `th:text`를 사용할 수도 있지만, `[[]]` 문법을 사용하면 더욱 간편하게 처리할 수 있습니다.

---

## `th:utext`

`th:text`는 데이터를 문자 그대로 출력하기 때문에, 컨트롤러에서 HTML 태그를 포함한 문자열을 전달하면 HTML 태그가 이스케이프(escape)되어 화면에 출력됩니다.

### 예제 코드

```java
model.addAttribute("data", "Hello <b>Spring!</b>");
```

```html
<ul>  
    <li>th:text: <span th:text="${data}"></span></li>  
    <li>th:utext: <span th:utext="${data}"></span></li>  
</ul>

```

위와 같은 경우, `th:text`는 HTML 태그를 일반 텍스트로 변환하여 출력합니다.

### 화면 출력 결과

```html
<ul>
    <li>th:text: <span>Hello &lt;b&gt;Spring!&lt;/b&gt;</span></li>
    <li>th:utext: <span>Hello <b>Spring!</b></span></li>
</ul>
```

즉, `<b></b>` 태그를 HTML 태그로 해석하여 적용하려면 `th:utext`를 사용해야 합니다.

### `th:text` vs `th:utext` 차이점

|속성|HTML 태그 처리|용도|
|---|---|---|
|`th:text`|HTML 태그를 이스케이프하여 출력|안전한 일반 텍스트 출력|
|`th:utext`|HTML 태그를 실제 태그로 렌더링|HTML 태그 포함 문자열 출력|


> [!warning] 주의
> `th:utext`는 사용자의 입력을 직접 반영할 경우 보안상 XSS(사이트 간 스크립팅) 공격에 취약하기에 신뢰할 수 없는 데이터를 출력할 경우 주의해야 합니다.

