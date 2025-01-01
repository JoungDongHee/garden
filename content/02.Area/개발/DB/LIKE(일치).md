---
date: 2024 년 12 월 26 일 03 시 12 분
tags:
  - Dev
  - LIKE
  - LIKEIN
author: Joung Dong Hee
share: true
---

# LIKE(일치)

`LIKE` 연산자는 MySQL에서 문자열 패턴을 검색하는 데 사용됩니다. 주로 `WHERE` 절에서 특정 문자열 패턴과 일치하는 데이터를 찾기 위해 사용됩니다.

## 와일드카드 설명

1. **`%` (퍼센트):**
    - 0개 이상의 문자와 일치합니다.
    - 예: `"ab%"`는 `"ab"`, `"abc"`, `"abcd"`와 일치합니다.
2. **`_` (언더스코어):**
    - 정확히 1개의 문자와 일치합니다.
    - 예: `"a_d"`는 `"abd"`, `"a1d"`, `"a#d"`와 일치하지만, `"ad"`나 `"abcd"`와는 일치하지 않습니다.

# 예시 테이블: `users`

| id  | username  | email              |
|-----|-----------|--------------------|
| 1   | alice     | alice@example.com  |
| 2   | bob       | bob123@example.com |
| 3   | carol     | carol_test@example.com |
| 4   | david     | david123@example.com |
| 5   | john_doe  | john@example.com   |


## `%` 사용

"a"로 시작하는 모든 문자열 을 찾기 위해서는 다음과 같이 뒤에 `%` 를 붙여주면 된다. 반대로 `a` 로 끝나는 문자열은 앞에 퍼센트를 붙여주면된다. `%a`

```sql
SELECT * FROM users WHERE username LIKE 'a%';
```

|id|username|email|
|---|---|---|
|1|alice|alice@example.com|

## `_` 사용

첫 글자가 "b"이고, 세 번째 글자가 "b"인 3글자 문자열 을 찾기 위한 쿼리는 다음과 같다. `_` 는 하나의 문자열을 의미 하기 때문에 `boob` 와 같이 두글자 이상이 될 경우 찾을수 없다.

```sql
SELECT * FROM users WHERE username LIKE 'b_b';
```


|id|username|email|
|---|---|---|
|2|bob|bob123@example.com|


## `%`와 `_` 조합

두 번째 글자 이후에 "d"가 포함된 문자열 을 찾는 쿼리 이다.

```sql
SELECT * FROM users WHERE username LIKE '%_d%';
```

|id|username|email|
|---|---|---|
|4|david|david123@example.com|
|5|john_doe|john@example.com|


# 주의사항

1. **대소문자 구분:**
    
    - 기본적으로 MySQL은 대소문자를 구분하지 않습니다. (컬레이션이 `utf8_general_ci`일 경우)
    - 대소문자 구분이 필요한 경우 `BINARY` 키워드를 사용합니다.

```sql
SELECT * FROM users WHERE username LIKE BINARY 'A%';
```

2. **특수문자:**
    - 패턴에 `%`나 `_`를 포함하려면 `ESCAPE`를 사용합니다.

```sql
SELECT * FROM users WHERE username LIKE '%\_%' ESCAPE '\';
```
3. **LIKE IN은 없는 구문입니다**:
    
    - `LIKE`와 `IN`은 함께 사용할 수 없습니다.
    - `IN`은 고정된 값의 집합에 대해 사용되고, `LIKE`는 패턴 매칭을 수행하기 때문입니다.
    - 예를 들어 아래와 같은 구문은 **유효하지 않습니다**:
```sql
SELECT * FROM users WHERE username LIKE IN ('%alice%', '%bob%');
```

올바른 쿼리 는 다음과 같다.

```sql
SELECT * FROM users  WHERE username LIKE '%alice%' OR username LIKE '%bob%';
```

4. **비교적 느린 속도:**
    
    - `LIKE` 문에서 `%` 또는 `_` 와 같은 와일드카드를 사용할 경우, 인덱스를 효과적으로 사용할 수 없습니다.
    - 특히 앞에 와일드카드가 있는 경우(`'%abc'`), 데이터의 Full Table Scan이 발생할 수 있어 성능 저하를 초래합니다.

----
# 참고

https://www.w3resource.com/mysql/string-functions/mysql-like-function.php