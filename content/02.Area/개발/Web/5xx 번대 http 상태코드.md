---
date: 2024 년 10 월 07 일 23 시 10 분
tags:
  - Dev
  - Http
  - statuscode
share: true
---

# 5xx 

5xx번대 상태코드는 클라이언트의 요청이 아닌 서버 측의 문제로 인해 발생하는 상태코드입니다. 이 오류들은 서버가 클라이언트의 요청을 처리할 수 없거나, 처리를 완료하지 못한 경우를 나타냅니다.

## 500 : Internal Server Error

서버에서 어떠한 이유로 인해 클라이언트의 요청을 정상적으로 처리하지 못하였을 경우 발생하는 에러 이다.

응답 본문에는 서버의 상태 와 발생한 원인에 대해 설명이 포함되어 있다.

**`Internal Server Error` 로 발생한 에러에 대해 클라이언트에게는 구체적인 오류 원인을 공개하지 않습니다. 이는 보안상 이유로 서버 내부 정보가 노출되지 않도록 하는 것이 좋기 때문입니다.**

서버 개발자는 이 오류를 적절하게 처리해 클라이언트에게는 일반적인 오류 메시지만 제공하고, 세부적인 오류는 서버 로그에 기록하는 방식으로 대처하는 것이 좋습니다

### Request

```http
GET /highlights HTTP/1.1
Host: example.com
User-Agent: curl/8.6.0
Accept: */*

```


### Reponse

```http
HTTP/1.1 500 Internal Server Error
Content-Type: text/html;
Content-Length: 123

<!doctype html>
<html lang="en">
<head>
  <title>500 Internal Server Error</title>
</head>
<body>
  <h1>Internal Server Error</h1>
  <p>The server was unable to complete your request. Please try again later.</p>
  <p>If this problem persists, please <a href="https://example.com/support">contact support</a>.</p>
  <p>Server logs contain details of this error with request ID: ABC-123.</p>
</body>
</html>

```


## 503 :  Service Unavailable

서버의 일시적인 과부하 혹은 점검등으로 인해 요청을 처리할수 없는 경우 서버에서 `503` 에러를 발생시키며 이 경우 클라이언트는 503 에러 페이지로 이동한다.

응답 본문에서는 현재 서버의 상태를 식별할수 있도록 내용을 포함해야한다. 

또한 응답에는 `Retry-After: 120` 와 같이 헤더를 추가하여 언제 다시 요청을 보내면 되는지 에 대해 예상 복구 시간을 명시하는 것이 좋다.

### Request

```http
GET /highlights HTTP/1.1
Host: example.com
User-Agent: curl/8.6.0
Accept: */*

```


### Reponse

```http
HTTP/1.1 503 Service Unavailable
Content-Type: text/html;
Content-Length: 123
Retry-After: 120


<!doctype html>
<html lang="en">
<head>
  <title>503 Service Unavailable</title>
</head>
<body>
  <h1>503 Service Unavailable</h1>
  <p>The server was unable to complete your request. Please try again later.</p>
  <p>If this problem persists, please <a href="https://example.com/support">contact support</a>.</p>
  <p>Server logs contain details of this error with request ID: ABC-123.</p>
</body>
</html>

```