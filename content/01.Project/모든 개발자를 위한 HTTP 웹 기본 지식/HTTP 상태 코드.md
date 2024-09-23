---
create: 2023-12-31
dg-publish: true
tags:
  - dev
  - Http
  - State
  - Code
---

# HTTP 상태 코드

* **클라이언트**가 보낸 요청 사항에 대해 서버에서 **처리 상태**를 알려주는 기능
* 응답 코드 **200**, **404** 이 대표적인 예시

## 2xx (Sucessful)
* **성공적으로 처리되었음을 나타내는 상태 코드**
### 200 : OK 
* 요청이 성공적으로 처리되었음을 나타냄
### 201 : Create
* 요청이 성공적으로 처리되었으며 새로운 Resource 가 **생성** 되었음을 나타냄
* 반환 된 응답 데이터에 새로 생성된 URL(**Location**) 이 포함됨
```
HTTP/1.1 201 Created
Content-Type: application/json
Content-Length: 123
Location: https://api.example.com/resource/1

{
  "status": "success",
  "data": {
    "id": 1,
    "name": "John Doe",
    "email": "john.doe@example.com"
  }
}
```
### 202 : Accepted
* 요청이 접수되었으나 아직 처리 중 일 때 사용함
* ex) 주문 접수 후 1시간 뒤 에 처리가 완료됨

### 204 : No Content
* 서버가 요청을 성공적으로 수행했지만 , 응답 결과 본문에 보낼 데이터가 없음 
* ex) 문서를 수정 및 저장 하여 Save 버튼을 클릭하여 저장 완료 했지만 페이지 이동 및 현재 페이지를 유지 해야한다.
### 205 : Reset Content
* 요청이 성공적으로 처리되었으며 , 현재 사용자가 보고 있는 문서를 리셋 할때 사용
* ex) 사용자가 입력중 인 폼 화면을 초기화 할때 사용

## 3xx (Redirection)

^159115

* 요청을 완료하려면 추가적인 행동이 필요
* 3xx 번대 응답 결과에 Locaion 헤더가 존재 할 경우 해당 Location URL 로 자동 이동 시킨다.
``` 
HTTP/1.1 301 Moved Permanently 
Location: https://www.example.com/new-location 
Content-Length: 0 
```

## 4xx (Client Error)
* 클라이언트 오류 , 잘못된 문법 등으로 서버에서 요청을 수행할 수 없을 경우 발생
* 클라이언트 에 서 잘못된 요청을 보내고 있기 때문에 재시도 해도 오류가 발생함
* **404** 의 Not Found 가 대표적인 예시 

### 400 : Bad Request
* 클라이언트 에서 서버로 **잘못된 요청을 전송**해서 처리할 수 없는 경우발생
* ex) 잘못된 요청 Parameter , API 스펙 과 맞지 않는 경우

### 401 : Unauthorized
* 잘못된 인증 일 경우 발생 
* 클라이언트가 해당 리소스 에 접근 하기 위해서는 **인증이 필요함**
* ex) 본인만이 접근 가능한 정보에 다른 사람이 접근 할 경우 , Admin 권한이 필요한 경우
### 403 : ForBidden
* 서버가 요청을 이해했지만 **승인을 거부함**
* ex) 사용자가 로그인을 했지만 Admin 권한 등급의 정보에 접근할려고 하는 경우 
### 404 : Not Found
* 요청 Resource 를 **찾을 수 없음**
* 요청 리소스가 서버에 없음 
* 잘못된 URL 에 요청을 보내는 경우 

## 5xx (Server Error)

* 서버에서 **알수없는 이유로 응답데이터를 줄수 없는 경우** 발생
* 서버가 요청을 정상적으로 처리 할수 없는경우
### 501 : Internal Server Error

* 대표적인 서버 에러 코드
### 503 : Service Unavailable

* 서비스 의 이용 불가 
* 서비스가 일시적인 과부하 또는 예정된 작업(서버업데이트) 등 으로 요청을 처리할수 없음