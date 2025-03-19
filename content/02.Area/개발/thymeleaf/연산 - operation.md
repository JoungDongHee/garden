---
date: 2025 년 03 월 19 일 16 시 03 분
tags:
  - Dev
  - operation
  - Thymeleaf
  - 연산
author: Joung Dong Hee
share: true
---
# Thymeleaf 연산자 (Operation)

Thymeleaf에서는 Java에서처럼 다양한 연산을 지원하며, 산술 연산, 비교 연산, 논리 연산, 조건 연산 등을 사용할 수 있습니다.

## 1. 산술 연산 (Arithmetic Operations)

Thymeleaf는 기본적인 사칙 연산을 지원하며, 문자열 연결에도 `+` 연산자를 사용할 수 있습니다.

|연산자|설명|
|---|---|
|`+`|덧셈 및 문자열 연결|
|`-`|뺄셈|
|`*`|곱셈|
|`/`|나눗셈|
|`%`|나머지|

```html
<ul>  
    <li>10 + 2 = <span th:text="10 + 2"></span></li>  
    <li>10 - 2 = <span th:text="10 - 2"></span></li>  
    <li>10 * 2 = <span th:text="10 * 2"></span></li>  
    <li>10 / 2 = <span th:text="10 / 2"></span></li>  
    <li>10 % 2 = <span th:text="10 % 2"></span></li>  
    <li>'Hello ' + 'World' = <span th:text="'Hello ' + 'World'"></span></li>
</ul> 

```

**출력 결과:**

```html
<li>10 + 2 = <span>12</span></li>
<li>10 - 2 = <span>8</span></li>
<li>10 * 2 = <span>20</span></li>
<li>10 / 2 = <span>5</span></li>
<li>10 % 2 = <span>0</span></li>
<li>'Hello ' + 'World' = <span>Hello World</span></li>

```

## 2. 비교 연산 (Comparison Operations)

Thymeleaf는 숫자 및 문자열 비교를 위한 다양한 연산을 지원하며, HTML 특성상 `<`, `>` 연산자는 `&lt;`, `&gt;`로 변환해야 합니다.

| 연산자  | 대체 연산자      | 설명                    |
| ---- | ----------- | --------------------- |
| `>`  | `gt`        | 초과 (greater than)     |
| `<`  | `lt`        | 미만 (less than)        |
| `>=` | `ge`        | 이상 (greater or equal) |
| `<=` | `le`        | 이하 (less or equal)    |
| ==   | `eq`        | 같음 (equal)            |
| `!=` | `neq`, `ne` | 다름 (not equal)        |

```html
<ul>  
    <li>1 > 10 = <span th:text="1 > 10"></span></li>  
    <li>1 gt 10 = <span th:text="1 gt 10"></span></li>  
    <li>1 >= 10 = <span th:text="1 >= 10"></span></li>  
    <li>1 ge 10 = <span th:text="1 ge 10"></span></li>  
    <li>1 == 10 = <span th:text="1 == 10"></span></li>  
    <li>1 != 10 = <span th:text="1 != 10"></span></li>  
    <li>'apple' == 'Apple' = <span th:text="'apple' == 'Apple'"></span></li>
</ul>
```

**출력 결과:**

```html
<li>1 > 10 = <span>false</span></li>
<li>1 gt 10 = <span>false</span></li>
<li>1 >= 10 = <span>false</span></li>
<li>1 ge 10 = <span>false</span></li>
<li>1 == 10 = <span>false</span></li>
<li>1 != 10 = <span>true</span></li>
<li>'apple' == 'Apple' = <span>false</span></li>
```

> [!info] 참고
> 문자열 비교 시 대소문자를 구분하므로, `"apple" == "Apple"`은 `false`를 반환합니다.

## 3. 논리 연산 (Logical Operations)

Thymeleaf는 `&&`, `||`, `!` 등의 논리 연산자를 지원하며, `and`, `or`, `not`을 대체 연산자로 사용할 수 있습니다.

```html
<ul>
    <li>true && false = <span th:text="true && false"></span></li>
    <li>true || false = <span th:text="true || false"></span></li>
    <li>!true = <span th:text="!true"></span></li>
    <li>true and false = <span th:text="true and false"></span></li>
    <li>true or false = <span th:text="true or false"></span></li>
    <li>not true = <span th:text="not true"></span></li>
</ul>

```

## 4. 조건 연산 (Conditional Expression)

Thymeleaf는 삼항 연산자를 사용하여 조건에 따라 값을 다르게 출력할 수 있습니다.

```html
<ul>  
    <li>(10 % 2 == 0) ? '짝수' : '홀수' = <span th:text="(10 % 2 == 0) ? '짝수' : '홀수'"></span></li> 
</ul>
```

**출력 결과:**

```html
<li>(10 % 2 == 0) ? '짝수' : '홀수' = <span>짝수</span></li>
```

## 5. Elvis 연산자 (Elvis Operator)

Elvis 연산자는 null 체크 및 기본값 지정을 간편하게 처리할 수 있습니다.

```html
<ul>  
    <li>${data} ?: '데이터가 없습니다.' = <span th:text="${data} ?: '데이터가 없습니다.'"></span></li>  
    <li>${nullData} ?: '데이터가 없습니다.' = <span th:text="${nullData} ?: '데이터가 없습니다.'"></span></li>  
</ul>

```
## 6. 컬렉션 연산 (Collection Operations)

Thymeleaf에서는 컬렉션 관련 연산도 가능합니다.

```html
<ul>
    <li>리스트 크기: <span th:text="${list.size()}"></span></li>
    <li>리스트가 비어 있는가? <span th:text="${list.isEmpty()}"></span></li>
    <li>'apple' 포함 여부: <span th:text="${list.contains('apple')}"></span></li>
</ul>

```

## 7. No-Operation (`_`)

Thymeleaf의 `_`는 연산을 수행하지 않고 태그 내부의 기본값을 그대로 유지하는 역할을 합니다.

```html
<ul>
    <li>${data} ?: _ = <span th:text="${data} ?: _">데이터가 없습니다.</span></li>  
    <li>${nullData} ?: _ = <span th:text="${nullData} ?: _">데이터가 없습니다.</span></li>  
</ul>
```

**출력 결과:**
- `${data}`가 값이 있으면 해당 값이 출력됨.
- `${nullData}`가 `null`이면 태그 내부의 `"데이터가 없습니다."`가 유지됨.