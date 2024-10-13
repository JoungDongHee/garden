---
date: 2024 년 10 월 06 일 22 시 10 분
tags:
  - Dev
  - statuscode
  - Http
share: true
---

# 2xx 

 2xx 번대 상태코드는 요청이 성공적으로 처리되었음을 의미 한다.

## 200 : OK

* 요청 성공 

다음과 같이 `/subscribe` API 에 POST 요청을 보낼 경우 정상 처리될 경우 `Reponse` 에는 200 이라는 HTTP 상태 코드 와 함께 부가적인 정보를 전달해준다.
### Request
```Http
POST /subscribe HTTP/1.1
Host: example.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 50

name=Brian%20Smith&email=brian.smith%40example.com

```
### Reponse
```http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "message": "User subscription pending. A confirmation email has been sent.",
  "subscription": {
    "name": "Brian Smith",
    "email": "brian.smith@example.com",
    "id": 123,
    "feed": "default"
  }
}

```


## 201 : Create

* 요청이 정상 처리되어 새로운 리소스가 생성됨

다음과 같이 유저를 생성할 경우 응답코드에는 새로 생성된 리소스 정보를 `Location` 속성 과 정보를 전달해준다.

### Request

```
POST /users HTTP/1.1
Host: example.com
Content-Type: application/json

{
  "firstName": "Brian",
  "lastName": "Smith",
  "email": "brian.smith@example.com"
}


```


### Reponse

```Http
HTTP/1.1 201 Created
Content-Type: application/json
Location: http://example.com/users/123

{
  "message": "New user created",
  "user": {
    "id": 123,
    "firstName": "Brian",
    "lastName": "Smith",
    "email": "brian.smith@example.com"
  }
}

```

## 202 : Accepted

* 요청이 접수 되었으나 아직 처리가 완료되지 않음
* 배치 처리와 같은 곳 에 사용된다
* 요청의 실제 처리(**성공 , 실패** )가 보장되지 않음

예를들어 다음과 같이 반려견 주인에게 픽업간다는 작업을 이메일로 보내는 배치 로직이 있을경우 `Reponse` 에는 시작하는 요청이 처리되었음을 나타내고 현재의 상태를 추적하기 위한 url 을 전달해준다 (`monitorUrl`) 

### Request

```Http
POST /tasks HTTP/1.1
Host: example.com
Content-Type: application/json

{
  "task": "emailDogOwners",
  "template": "pickup"
}

```


### Response

```http
HTTP/1.1 202 Accepted
Date: Wed, 26 Jun 2024 12:00:00 GMT
Server: Apache/2.4.1 (Unix)
Content-Type: application/json

{
  "message": "Request accepted. Starting to process task.",
  "taskId": "123",
  "monitorUrl": "http://example.com/tasks/123/status"
}

```

## 204 :  No Content 

서버가 요청을 성공적으로 수행 했으나 클라이언트가 현재 페이지에서 이동할 필요가 없음을 나타낸다. 

다음과 같이 유저가 DELET 메소드를 통해 이미지를 삭제하고자 한다고 가정할 경우 이미지를 성공적으로 삭제한 후에는 서버에서는 204 상태 코드만 전달  해도 성공으로 인식할수 있다.

### Request 

```http
DELETE /image/123 HTTP/1.1
Host: example.com
Authorization: Bearer 1234abcd

```

### Response 

```Http
HTTP/1.1 204 No Content
Date: Wed, 26 Jun 2024 12:00:00 GMT
Server: Apache/2.4.1 (Unix)
Content-Length: 0

```


# 그 외 

그 외 에도 다양한 2xx 번대의 상태코드가 존재하나 대체적으로 위에 사용하는 상태코드가 보편적이다.

--- 
# 참고

https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/200

