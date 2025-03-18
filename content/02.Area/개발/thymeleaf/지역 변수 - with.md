---
date: 2025 년 03 월 18 일 12 시 03 분
tags:
  - Dev
  - with
  - 지역변수
  - Thymeleaf
author: Joung Dong Hee
share: true
---

# 지역 변수 - with

## 기본 사용법

예를 들어, 컨트롤러에서 `users` 리스트를 모델에 추가했다고 가정해 보겠습니다.

```java
model.addAttribute("users", list);  // 리스트 추가
```

다음과 같이 `th:with`를 사용하여 `users[0]`을 `first`라는 지역 변수로 선언하면, 해당 태그 내부에서 `first`를 통해 `users[0]`을 참조할 수 있습니다.

```html
<div th:with="first=${users[0]}">
    <p>처음 사람의 이름은 <span th:text="${first.username}"></span></p>
</div>
```

## `th:with`를 중첩해서 사용

`th:with`는 여러 개의 태그에서 중첩하여 사용할 수도 있습니다.

```html
<div th:with="first=${users[0]}">
    <p>처음 사람: <span th:text="${first.username}"></span></p>
    <div th:with="isAdult=${first.age >= 18}">
        <p>성인 여부: <span th:text="${isAdult}"></span></p>
    </div>
</div>

```

### 중요한 점

1. `th:with`로 선언된 변수는 해당 태그 내부에서만 유효합니다. 즉, `div` 태그 범위를 벗어나면 `first` 변수를 사용할 수 없습니다.
2. `th:with`는 하나의 변수만 선언하는 것이 아니라, 여러 개의 변수를 `쉼표(,)`로 구분하여 선언할 수도 있습니다.

```html
<div th:with="first=${users[0]}, last=${users[users.size() - 1]}">
    <p>처음 사람: <span th:text="${first.username}"></span></p>
    <p>마지막 사람: <span th:text="${last.username}"></span></p>
</div>
```