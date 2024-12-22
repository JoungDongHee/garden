---
date: 2024 년 12 월 12 일 16 시 12 분
tags:
  - Dev
  - DB
  - UNION
  - UNIONALL
  - 테이블병합
author: Joung Dong Hee
share: true
---

# UNION

여러개의 `SELECT` 쿼리의 결과를 하나로 결합하여 결과를 만들때 사용한다. 결과를 합칠때 중복된 행은 제거 하며 중복된 행을 포함할때는 [ UNION ALL](UNION%20(%ED%85%8C%EC%9D%B4%EB%B8%94%20%EB%B3%91%ED%95%A9)%20%EA%B3%BC%20UNION%20ALL.md#^bc0300) 을 사용해야한다.

`UNION` 과  `UNION ALL` 은 둘다 컬럼의  갯수 와 타입이 같아야만 사용이 가능한 문법이다. 이를 어길시 문법 오류가 발생한다.[참고](UNION%20(%ED%85%8C%EC%9D%B4%EB%B8%94%20%EB%B3%91%ED%95%A9)%20%EA%B3%BC%20UNION%20ALL.md#^7fa72a)

`UNION` 은 내부적으로 중복 제거를 하는 로직이 들어가기 때문에 중복제거를 할 필요가 없다면 `UNION ALL` 을 사용하는게 효율적이다.

## **예제 테이블**

### `employees` 테이블

|id|name|position|
|---|---|---|
|1|Alice|Developer|
|2|Bob|Manager|
|3|Charlie|Developer|

### `contractors` 테이블

|id|name|position|
|---|---|---|
|101|David|Designer|
|102|Eva|Developer|
|103|Bob|Manager|

```sql
SELECT name, position FROM employees
UNION
SELECT name, position FROM contractors;

```


### 결과:

|name|position|
|---|---|
|Alice|Developer|
|Bob|Manager|
|Charlie|Developer|
|David|Designer|
|Eva|Developer|

# UNION ALL

`UNION` 과 다르게 중복된 행을 포함한 결과를 반환하는 문법이이다. ^bc0300


## **예제 테이블**

### `employees` 테이블

|id|name|position|
|---|---|---|
|1|Alice|Developer|
|2|Bob|Manager|
|3|Charlie|Developer|

### `contractors` 테이블

|id|name|position|
|---|---|---|
|101|David|Designer|
|102|Eva|Developer|
|103|Bob|Manager|



```sql
SELECT name, position FROM employees
UNION ALL
SELECT name, position FROM contractors;

```

## 결과:

|name|position|
|---|---|
|Alice|Developer|
|Bob|Manager|
|Charlie|Developer|
|David|Designer|
|Eva|Developer|
|Bob|Manager|


## 컬럼이 다른 테이블 결합

^7fa72a

컬럼 수와 데이터 유형을 맞추기 위해 추가 작업을 해야 한다.. 예를 들어, `contractors` 테이블에 없는 `salary` 정보를 `NULL`로 채우는 경우

```sql
SELECT id, name, position, salary FROM employees
UNION
SELECT id, name, position, NULL AS salary FROM contractors;

```

### `employees` 테이블

|id|name|position|salary|
|---|---|---|---|
|1|Alice|Developer|6000|
|2|Bob|Manager|8000|
|3|Charlie|Developer|6500|


### 결과:

|id|name|position|salary|
|---|---|---|---|
|1|Alice|Developer|6000|
|2|Bob|Manager|8000|
|3|Charlie|Developer|6500|
|101|David|Designer|NULL|
|102|Eva|Developer|NULL|
|103|Bob|Manager|NULL|