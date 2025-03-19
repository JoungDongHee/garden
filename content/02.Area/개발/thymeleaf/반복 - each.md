---
date: 2025 년 03 월 19 일 18 시 03 분
tags:
  - Dev
  - each
  - 반복
  - Thymeleaf
author: Joung Dong Hee
share: true
---
Thymeleaf에서는 `java.util.Iterable` 및 `java.util.Enumeration`을 구현한 객체(`List`, `Map`, `Set` 등)를 반복하여 화면에 중복 코드 없이 표현할 수 있다.

## 1. `th:each` 기본 사용법

예제로 `User` 객체를 `List` 형태로 반환하여 컨트롤러에서 모델에 추가하는 코드이다.

```java
List<User> list = new ArrayList<>();  
list.add(new User("userA", 10));  
list.add(new User("userB", 20));  
list.add(new User("userC", 30));  
model.addAttribute("users", list);
```


Thymeleaf 템플릿에서는 `th:each` 문법을 사용하여 리스트를 반복 렌더링할 수 있다.

```html
<tr th:each="user : ${users}">
    <td th:text="${user.username}">username</td>
    <td th:text="${user.age}">0</td>
</tr>
```

### 렌더링 결과

`users` 리스트에 3개의 객체가 있으므로 `th:each` 문이 실행될 때 `tr` 태그가 3번 생성된다.


```html
<tr>
    <td>userA</td>
    <td>10</td>
</tr>
<tr>
    <td>userB</td>
    <td>20</td>
</tr>
<tr>
    <td>userC</td>
    <td>30</td>
</tr>

```

## 2. 반복 상태 변수 (`Stat`)

`th:each`는 반복문 상태 정보를 제공하는 `Stat` 변수를 지원한다. 이 변수를 사용하면 현재 반복 상태(인덱스, 개수, 첫 번째 항목 여부, 마지막 항목 여부 등)를 쉽게 확인할 수 있다.


```html
<tr th:each="user, userStat : ${users}">
    <td>
        index = <span th:text="${userStat.index}"></span>
        count = <span th:text="${userStat.count}"></span>
        size = <span th:text="${userStat.size}"></span>
        even? = <span th:text="${userStat.even}"></span>
        odd? = <span th:text="${userStat.odd}"></span>
        first? = <span th:text="${userStat.first}"></span>
        last? = <span th:text="${userStat.last}"></span>
        current = <span th:text="${userStat.current}"></span>
    </td>
</tr>
```

### 렌더링 결과

```html
<tbody>
    <tr>
        <td>
            index = <span>0</span>
            count = <span>1</span>
            size = <span>3</span>
            even? = <span>false</span>
            odd? = <span>true</span>
            first? = <span>true</span>
            last? = <span>false</span>
            current = <span>BasicController.User(username=userA, age=10)</span>
        </td>
    </tr>
    <tr>
        <td>
            index = <span>1</span>
            count = <span>2</span>
            size = <span>3</span>
            even? = <span>true</span>
            odd? = <span>false</span>
            first? = <span>false</span>
            last? = <span>false</span>
            current = <span>BasicController.User(username=userB, age=20)</span>
        </td>
    </tr>
    <tr>
        <td>
            index = <span>2</span>
            count = <span>3</span>
            size = <span>3</span>
            even? = <span>false</span>
            odd? = <span>true</span>
            first? = <span>false</span>
            last? = <span>true</span>
            current = <span>BasicController.User(username=userC, age=30)</span>
        </td>
    </tr>
</tbody>
****
```


### `Stat` 객체가 제공하는 속성

|속성명|설명|
|---|---|
|`index`|0부터 시작하는 반복 인덱스|
|`count`|1부터 시작하는 반복 횟수|
|`size`|전체 데이터 개수|
|`even`|현재 반복이 짝수인지 여부 (`true` 또는 `false`)|
|`odd`|현재 반복이 홀수인지 여부 (`true` 또는 `false`)|
|`first`|현재 반복이 첫 번째 요소인지 여부|
|`last`|현재 반복이 마지막 요소인지 여부|
|`current`|현재 요소의 데이터 값|

## 3. `Map` 데이터 반복

`th:each`는 `Map` 타입 데이터도 반복할 수 있다.

```java
Map<String, String> userMap = new LinkedHashMap<>();
userMap.put("userA", "Alice");
userMap.put("userB", "Bob");
userMap.put("userC", "Charlie");
model.addAttribute("userMap", userMap);
```

템플릿에서 `th:each`를 사용하여 `Map`을 반복하면 `key, value`를 동시에 접근할 수 있다.


```html
<tr th:each="entry : ${userMap}">
    <td th:text="${entry.key}"></td>
    <td th:text="${entry.value}"></td>
</tr>
```


### 렌더링 결과

```html
<tr>
    <td>userA</td>
    <td>Alice</td>
</tr>
<tr>
    <td>userB</td>
    <td>Bob</td>
</tr>
<tr>
    <td>userC</td>
    <td>Charlie</td>
</tr>

```



## 4. `Set` 데이터 반복

`Set`도 `th:each`로 반복할 수 있으며, 순서는 `Set`의 구현체에 따라 다를 수 있다.


```java
Set<String> names = new LinkedHashSet<>();
names.add("Alice");
names.add("Bob");
names.add("Charlie");
model.addAttribute("names", names);
```


템플릿에서 사용 예시:

```html
<ul>
    <li th:each="name : ${names}" th:text="${name}"></li>
</ul>
```

### 렌더링 결과


```html
<ul>
    <li>Alice</li>
    <li>Bob</li>
    <li>Charlie</li>
</ul>
```


## 5. `List` 데이터의 일부만 반복

Thymeleaf는 `|`(파이프) 연산자를 사용하여 리스트의 일부만 반복할 수 있다.

```html
<tr th:each="user : ${users[0..1]}">
    <td th:text="${user.username}"></td>
    <td th:text="${user.age}"></td>
</tr>
```


위 코드에서 `users[0..1]`는 인덱스 `0`부터 `1`까지의 요소만 반복한다.

### 렌더링 결과

```html
<tr>
    <td>userA</td>
    <td>10</td>
</tr>
<tr>
    <td>userB</td>
    <td>20</td>
</tr>
```


## 6. 홀수/짝수 스타일 적용

짝수 행과 홀수 행의 배경색을 다르게 적용할 때 `th:class`와 `userStat.even` 또는 `userStat.odd`를 사용할 수 있다.


```html
<tr th:each="user, userStat : ${users}" th:class="${userStat.even} ? 'even-row' : 'odd-row'">
    <td th:text="${user.username}"></td>
    <td th:text="${user.age}"></td>
</tr>
```


### CSS 예시

```css
.even-row { background-color: #f2f2f2; }
.odd-row { background-color: #ffffff; }
```