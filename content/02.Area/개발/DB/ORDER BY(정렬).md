---
date: 2024 년 11 월 26 일 09 시 11 분
tags:
  - Dev
  - DB
  - ORDERBY
  - SQL
author: Joung Dong Hee
share: true
finish: true
---

# ORDER BY(정렬)


DB 에서 데이터를 조회후 `오름차순 정렬` `내림차순 정렬` 등을 사용하기 위해서는 **Order BY** 라는 키워드를 사용해서 정렬을 해야 한다.

* 내림차순 :  4 -> 3  -> 2  -> 1 순서로 값이 큰 곳에서 값이 작은 곳으로 내려간다.
* 오름차순 : 1 -> 2 -> 3 -> 4 순서로 값이 작은 곳에서 값이 큰 곳으로 값이 올라간다.

## 내림차순 정렬 

```sql
SELECT * FROM employees ORDER BY salary DESC;
```

|id|name|salary|hire_date|
|---|---|---|---|
|4|Daisy|60000.00|2019-06-10|
|1|Alice|55000.00|2021-05-15|
|3|Charlie|52000.00|2020-11-20|
|2|Bob|48000.00|2022-03-12|
## 오름 차순 정렬 

```sql
SELECT * FROM employees ORDER BY salary ASC;
```

|id|name|salary|hire_date|
|---|---|---|---|
|2|Bob|48000.00|2022-03-12|
|3|Charlie|52000.00|2020-11-20|
|1|Alice|55000.00|2021-05-15|
|4|Daisy|60000.00|2019-06-10|

## 다중 정렬 

복수의 컬럼을 지정하여 정렬하는 방법으로 `hire_date` 컬럼을 기준으로  먼저 오름 차순 정렬한뒤

`salary` 컬럼을 내림 차순 정렬한다.

```sql
SELECT * FROM employees ORDER BY hire_date ASC, salary DESC;
```

| id  | name    | salary   | hire_date  |
| --- | ------- | -------- | ---------- |
| 4   | Daisy   | 60000.00 | 2019-06-10 |
| 3   | Charlie | 52000.00 | 2020-11-20 |
| 1   | Alice   | 55000.00 | 2021-05-15 |
| 2   | Bob     | 48000.00 | 2022-03-12 |

# Order BY 가 없을 경우 정렬 기준

만약 사용자가 `Order BY` 키워드 없이 쿼리를 실행할 경우 DB 내부에서는 어떠한 기준으로 정렬을 할까

```sql
SELECT * FROM employees
```

[Mysql 공식답변](https://forums.mysql.com/read.php?21,239471,239688)  에 따르면 다음과 같이 설명을 해주고 있다. 

>* Do not depend on order when ORDER BY is missing.  
>* Always specify ORDER BY if you want a particular order -- in some situations the engine can eliminate the ORDER BY because of how it does some other step.  
>* GROUP BY forces ORDER BY. (This is a violation of the standard. It can be avoided by using ORDER BY NULL.)


위 내용에 따르면 Mysql 내부에서 `PRIMARY KEY(PK)` 순서 와 데이터의 삽입 순서에 따라 반환될수 있다. 

하지만 데이터 의 수정 및 삭제 그리고 병합 작업 등 의 영향을 받을수 있기 때문의 데이터의 일관성을 보장하기 어려우며 정렬이 필요한 경우 반드시 `ORDER BY` 를 사용하여 정렬해야 한다.


--- 

# 참고

https://forums.mysql.com/read.php?21,239471,239471#msg-239471

https://dev.mysql.com/doc/refman/8.4/en/order-by-optimization.html

