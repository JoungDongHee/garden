---
date: 2024 년 10 월 06 일 22 시 10 분
tags:
  - Dev
  - statuscode
  - Http
share: true
---

# 4xx

4xx 번대의 상태 코드는 클라이언트 오류 로 인해 발생하는 코드로 잘못된 문법 , 잘못된 인자 등 으로 인해 서버에서 정상적으로 요청을 처리할수 없는 경우 발생한다.

클라이언트에서 이미 잘못된 요청을 보내고 있음으로 계속 재시도를 해도 실패가 발생한다.

## 400 : Bad Request

다음과 같이 `/users` 를 통하여 유저를 생성 할경우 잘못된 문법으로 인해 `API` 와의 통신이 실패할 경우  서버에서는 `Bad Request` 를 응답으로 준다.
### Request

```HTTP
POST /users HTTP/1.1
Host: example.com
Content-Type: application/json
Content-Length: 38

{
  "email": "b@example.com
",
  "username": "b.smith"
}

```


### Reponse

```HTTP
HTTP/1.1 400 Bad Request
Content-Type: application/json
Content-Length: 71

{
  "error": "Bad request",
  "message": "Request body could not be read properly.",
}

```

## 401 : Unauthorized

^868a95

`Unauthorized`  는 사용자가 해당 리소스에 대한 인증을 필요로 할때 발생하는 에러이다.

다음과 같이  인증을 받지 못한 유저가 `/admin` API 와 통신을 할려고 할 경우 `401` 에러를 반환하며  응답에는 **WWW-Authnticate 헤더와 함께 인증 방법을 설명** 해야 한다.
### Request


```http
GET /admin HTTP/1.1
Host: example.com

```


### Reponse

```http
HTTP/1.1 401 Unauthorized
Date: Tue, 02 Jul 2024 12:18:47 GMT
WWW-Authenticate: Bearer

```


## 403 : ForBidden

위에서 언급한 [ 401 상태코드](4xx%20%EB%B2%88%EB%8C%80%20http%20%EC%83%81%ED%83%9C%EC%BD%94%EB%93%9C.md#^868a95%20) 와 비슷해 보이나. 차이점이 존재한다.

`Unauthorized` 는 인증 자체가 안되어 거절이된 케이스 이며 `ForBidden` 의 경우 인증은 되어 있으나 권한이 없어 불가한 상태를 나타낸다. 

예를들어 다음과 같이 `/users/123` 을 통하여 유저를 삭제 할려고 하지만 인증 정보는`abcd123` 이기 때문에 유저를 삭제할수 있는 권한이 없다.


### Request

```http
DELETE /users/123 HTTP/1.1
Host: example.com
Authorization: Bearer abcd123

```


### Reponse

```http
HTTP/1.1 403 Forbidden
Date: Tue, 02 Jul 2024 12:56:49 GMT
Content-Type: application/json
Content-Length: 88

{
  "error": "InsufficientPermissions",
  "message": "Deleting users requires the 'admin' role."
}

```


## 404 : Not Found

웹 개발에서 가장 흔하게 발생하는 오류 중 하나로 문자 그대로 `Not Found` 즉 리소스를 찾을수 없다 라는 의미 이다.

`JSP` 를 통한 개발 과정에서도 자주 접할수 있는 메시지 이며 잘못된 리디렉션을 통해서도 발생한다.

### Request

```http
GET /my-deleted-blog-post HTTP/1.1
Host: example.com

```


### Reponse

```http
HTTP/1.1 404 Not Found
Age: 249970
Cache-Control: max-age=604800
Content-Type: text/html; charset=UTF-8
Date: Fri, 28 Jun 2024 11:40:58 GMT
Expires: Fri, 05 Jul 2024 11:40:58 GMT
Last-Modified: Tue, 25 Jun 2024 14:14:48 GMT
Server: ECAcc (nyd/D13E)
Vary: Accept-Encoding
X-Cache: 404-HIT
Content-Length: 1256

<!doctype html>
<head>
    <title>404 not found</title>
    ...

```


## 405 : Method Not Allowed

`POST` 방식의 API 에 `GET` 요청을 통해 정보를 요청하는 경우와 같이 잘못된 `Method` 로 인해 발생하는 에러이다.


다음 요청 에서는 `example.com` 에서 `TRACE` 방식의 `Method` 를 허용하지 않는다는 의미이다. 

응답에서는 `Allow: GET, POST, HEAD` 와 같이 명시를 하여 사용가능한 `Method` 를 알려준다. 

### Request

```http
TRACE / HTTP/1.1
Host: example.com

```


### Reponse

```http
HTTP/1.1 405 Method Not Allowed
Content-Length: 0
Date: Fri, 28 Jun 2024 14:30:31 GMT
Server: ECLF (nyd/D179)
Allow: GET, POST, HEAD

```