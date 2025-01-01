---
date: 2024 년 12 월 12 일 15 시 12 분
tags:
  - Dev
  - DB
  - GROUPBY
  - SQL
author: Joung Dong Hee
share: true
---

# GROUP BY

데이터의 특정 열(컬럼)을 기준으로 그룹화할 때 사용되는 절입니다. GROUP BY 는 동일한 데이터를 가진 행 을 하나의 행으로 묶으며 이렇게 그룹화된 데이터는 보통 **집계 함수(Aggregate Function)** 와 함께 사용됩니다. 예를 들어, `SUM`, `AVG`, `COUNT`, `MAX`, `MIN` 등이 있습니다.

* HAVING 은 그룹화 하여 이미 만들어진 데이터에서 조건을 걸때 사용한다.
* WHERE 은 그룹화 하기 이전 데이터를 필터링 하는 방법이다.

## 예제 테이블

###  `sales` (판매 기록)

| sale_id | product | category  | price | sale_date  |
| ------- | ------- | --------- | ----- | ---------- |
| 1       | Apple   | Fruit     | 3     | 2024-12-01 |
| 2       | Banana  | Fruit     | 2     | 2024-12-01 |
| 3       | Carrot  | Vegetable | 1     | 2024-12-02 |
| 4       | Apple   | Fruit     | 3     | 2024-12-03 |
| 5       | Lettuce | Vegetable | 2     | 2024-12-03 |
| 6       | Banana  | Fruit     | 2     | 2024-12-03 |


### 예제 1: 제품별 판매 수량

```sql
SELECT product, COUNT(*) AS sale_count
FROM sales
GROUP BY product;
```
#### 결과:

|product|sale_count|
|---|---|
|Apple|2|
|Banana|2|
|Carrot|1|
|Lettuce|1|


### 예제 2: 카테고리별 총 판매 금액

```sql
SELECT category, SUM(price) AS total_revenue
FROM sales
GROUP BY category;

```

#### 결과:

|category|total_revenue|
|---|---|
|Fruit|10|
|Vegetable|3|


### 예제 3: 특정 판매 금액 초과 그룹 필터링 (HAVING 사용)

* 이미 그룹화 하여 만들어진 테이블에서 `SUM(price)` 값이 5 보다 큰 값을 찾는 다.
 
```sql
SELECT category, SUM(price) AS total_revenue
FROM sales
GROUP BY category
HAVING SUM(price) > 5;
```

#### 결과:

|category|total_revenue|
|---|---|
|Fruit|10|


### 예제 4: 날짜별 카테고리별 평균 가격

```sql
SELECT sale_date, category, AVG(price) AS avg_price
FROM sales
GROUP BY sale_date, category;

```

#### 결과:

|sale_date|category|avg_price|
|---|---|---|
|2024-12-01|Fruit|2.5|
|2024-12-02|Vegetable|1.0|
|2024-12-03|Fruit|2.5|
|2024-12-03|Vegetable|2.0|

---

# 참고

https://extbrain.tistory.com/56

https://youtu.be/6qkPy7RfLqQ