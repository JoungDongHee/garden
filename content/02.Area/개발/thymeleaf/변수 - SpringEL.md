---
date: 2025 년 03 월 18 일 11 시 03 분
tags:
  - Dev
  - SpringEL
  - Thymeleaf
  - 변수
author: Joung Dong Hee
share: true
---

# 변수 - SpringEL

Spring에서 전달한 데이터가 단순한 문자열이 아닌 객체일 경우, `SpringEL` (`${...}`) 문법을 사용하여 값을 출력할 수 있습니다.

## 1. 데이터 전달

Spring 컨트롤러에서 `Model`을 통해 데이터를 전달하는 방식은 다음과 같습니다.

```java
model.addAttribute("user", userA);  // 단일 객체
model.addAttribute("users", list);  // 리스트
model.addAttribute("userMap", map); // Map 객체
```


## 2. Thymeleaf에서 데이터 출력

HTML에서 `SpringEL`을 사용하여 데이터를 출력할 수 있습니다.


### 2.1 단일 객체 (Object)

`user`는 단일 객체이므로, 객체의 필드에 직접 접근할 수 있습니다.

```html
<ul>Object  
    <li>${user.username} = <span th:text="${user.username}"></span></li>  
    <li>${user['username']} = <span th:text="${user['username']}"></span></li>  
    <li>${user.getUsername()} = <span th:text="${user.getUsername()}"></span></li>  
</ul>  
```


### 2.2 리스트 (List)

`users`는 `List<User>` 구조이므로, 인덱스를 이용해 특정 객체에 접근한 후 필드 값을 출력할 수 있습니다.

```html
<ul>List  
    <li>${users[0].username} = <span th:text="${users[0].username}"></span></li>  
    <li>${users[0]['username']} = <span th:text="${users[0]['username']}"></span></li>  
</ul>  
```


### 2.3 맵 (Map)

`userMap`은 `Map<String, User>` 구조를 가지며, Key를 사용하여 데이터를 조회할 수 있습니다.

```html
<ul>Map  
    <li>${userMap['userA'].username} = <span th:text="${userMap['userA'].username}"></span></li>  
    <li>${userMap['userA']['username']} = <span th:text="${userMap['userA']['username']}"></span></li>  
</ul>

```