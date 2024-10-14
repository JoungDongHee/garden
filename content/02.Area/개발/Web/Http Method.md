---
date: 2024 년 10 월 14 일 23 시 10 분
tags:
  - Dev
author: Joung Dong Hee
share: true
---
# HTTP Method

HTTP에서 Method는 클라이언트가 서버에 요청을 보낼 때 그 요청의 목적을 정의하는 방식으로, 이를 통해 [REST API](REST%20API.md)를 설계할 수 있습니다.

HTTP Method에는 GET, POST, PUT, PATCH, DELETE, HEAD, OPTIONS, CONNECT, TRACE 등이 존재하며, 가장 많이 사용되는 것은 **GET, POST, PUT, PATCH, DELETE**입니다. 각 Method를 보고 클라이언트는 해당 API의 동작 방식(조회, 생성, 업데이트, 삭제 등)을 유추할 수 있습니다.

## 주요 개념

1. **멱등성(Idempotency)**: 동일한 요청을 여러 번 보내도 서버의 상태가 동일하게 유지되는 속성입니다. GET, HEAD, PUT, DELETE는 멱등성을 가집니다.

2. **안전성(Safety)**: 리소스를 변경하지 않는 속성입니다. GET과 HEAD는 안전한 메서드로 간주됩니다.

3. **CORS(Cross-Origin Resource Sharing)**: 일부 메서드는 CORS 정책에 따라 추가적인 처리가 필요할 수 있습니다.

## GET

* 리소스를 조회하는 Method입니다.
* 서버에 전달하고 싶은 데이터를 query를 통해 전달할 수 있습니다.
* GET 요청은 멱등성을 가집니다.
* Message Body를 통해 데이터를 전달하는 것은 가능하지만, 지원하지 않는 클라이언트가 많아 권장되지 않습니다.
* GET 요청은 캐시가 가능하며, 브라우저 히스토리에 남습니다.
* Query 문자열 길이 제한은 브라우저나 서버에 따라 다를 수 있습니다.

### Example
```http
GET https://m.entertain.naver.com/article/213/0001312836
```

## POST

* 요청 받은 데이터를 처리하는 Method입니다.
* Message Body를 통해 전달 받은 데이터를 API는 비즈니스 로직에 맞게 처리합니다.
* 주로 신규 리소스 생성('게시글 작성', '회원정보 생성' 등)에 사용되지만, 복잡한 연산을 요구하는 경우에도 사용됩니다.
* POST는 멱등성을 보장하지 않습니다.
* 일반적으로 캐시되지 않으며, 브라우저 히스토리에 남지 않습니다.

### Example
#### Request
```http
POST / HTTP/1.1
Host: foo.com
Content-Type: application/json
Content-Length: 34
{
	"username" : "jdh",
	"age" : 20
}
```
#### Response 
```http
HTTP/1.1 201 Created
Content-Type: application/json
{
	"username" : "jdh",
	"age" : 20,
	"memberNumber" : 10
}
```

## PUT

* 리소스를 완전히 대체하는 Method입니다.
* PUT은 멱등성을 가집니다.
* 주로 전체 리소스를 업데이트할 때 사용되며, 리소스가 없는 경우 새로 생성할 수도 있습니다.

> [!danger]+ Danger
> 기존의 리소스를 완전히 덮어 씌워 대체하는 Method이기 때문에 데이터 유실이 생길 수 있습니다.
> 
> 예를 들어:
> ```python
> # 기존 사용자 데이터
> existing_user = {
>     "id": 1,
>     "name": "홍길동",
>     "email": "hong@example.com",
>     "age": 30,
>     "address": "서울시 강남구"
> }
>
> # PUT 요청으로 보내는 새 데이터
> new_data = {
>     "name": "김철수",
>     "email": "kim@example.com"
> }
>
> # PUT 요청 후 결과 (전체 리소스가 대체됨)
> updated_user = {
>     "id": 1,
>     "name": "김철수",
>     "email": "kim@example.com"
> }
> 
> # 'age'와 'address' 필드가 유실되었음
> ```

### Example
#### Request
```http
PUT /new.html HTTP/1.1
Host: example.com
Content-type: text/html
Content-length: 16
<p>New File</p>
```
#### Response
```http
HTTP/1.1 201 Created
Content-Location: /new.html
```

## PATCH

* 리소스를 부분적으로 수정하는 Method입니다.
* PATCH는 상황에 따라 멱등성을 가질 수도, 가지지 않을 수도 있습니다.

> [!info]+ PATCH 메서드
> PATCH는 리소스의 부분적인 수정을 위해 사용되는 HTTP 메서드입니다. 리소스의 일부만 업데이트하므로 데이터 효율성이 높고 불필요한 덮어쓰기를 방지할 수 있습니다.
> 
> 예시:
> ```python
> # 기존 사용자 데이터
> existing_user = {
>     "id": 1,
>     "name": "홍길동",
>     "email": "hong@example.com",
>     "age": 30,
>     "address": "서울시 강남구"
> }
> 
> # PATCH 요청으로 보내는 수정 데이터
> patch_data = {
>     "name": "김길동",
>     "age": 31
> }
> 
> # PATCH 요청 후 결과 (지정된 필드만 업데이트됨)
> updated_user = {
>     "id": 1,
>     "name": "김길동",
>     "email": "hong@example.com",
>     "age": 31,
>     "address": "서울시 강남구"
> }
> 
> # 'name'과 'age' 필드만 업데이트되고 나머지는 유지됨
> ```

## DELETE

* 리소스를 삭제하는 Method입니다.
* DELETE도 멱등성을 가집니다. 같은 요청을 여러 번 보내도 결과는 동일합니다.

### Example
#### Request
```http
DELETE /file.html HTTP/1.1
```
#### Response
```http
HTTP/1.1 204 No Content
Date: Wed, 21 Oct 2015 07:28:00 GMT
```

## 기타 HTTP 메서드

* HEAD: GET과 유사하지만 응답 본문을 반환하지 않고 헤더만 반환합니다.
* OPTIONS: 서버가 지원하는 HTTP 메서드에 대한 정보를 요청합니다.

---
# 참조
* [[https://developer.mozilla.org/ko/docs/Web/HTTP/Methods]]
* 각 브라우저 및 서버의 공식 문서

