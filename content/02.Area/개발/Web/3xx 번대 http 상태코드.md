---
date: 2024 년 10 월 06 일 22 시 35 분
tags:
  - Dev
  - Http
  - statuscode
share: true
---
# 3xx 

 3xx 번대 상태코드는 요청이 완료되면 추가 클라이언트 에서 추가적인 행동이 필요한 경우 사용한다. 

예를 들어 `reponse` 에 `Location` 속성이 존재할 경우 해당 속성 의 url 로 [리다이렉션(Redirection)](%EB%A6%AC%EB%8B%A4%EC%9D%B4%EB%A0%89%EC%85%98(Redirection).md) 을 할수 있다.

# 영구 리다이렉션

특정 리소스의 URL 로 영구적으로 이동 하는 것을 말하며 원래의 URL 이 아닌 전혀 다른 URL 로 이동한다.

## 301 : Moved Permanently

^1e460b

리다이렉션 시 메시지 와 본문을 삭제한다. 다음 이미지 와 같이 `1.요청` POST 방식으로 `name=hello&age=20` 을 메시지에 담아 전달 하였으며 이후 리다이렉 션 `location : /new-event` 으로 페이지 이동 **자동으로 메소드가 GET 방식으로 변경된다.** 이때 본문에 메시지 와 내용은 전부 사라진 상태로 다시 요청을 보내게 된다.

![Pasted image 20231231215020.png](https://file-api.ksq9511.synology.me:5353/obsidian-image/20241103181650.png)



## 308 : Permanent Redirect 


[ 301](3xx%20%EB%B2%88%EB%8C%80%20http%20%EC%83%81%ED%83%9C%EC%BD%94%EB%93%9C.md#^1e460b%20) 과 기능은 동일하나 리다이렉트 요청시 메서드 와 본문을 유지한다.  다음과 같이 `1.요청` 에서 `POST` 방식으로 메시지를 전달하여 보낸 이후 `리다이렉션` 으로 페이지가 이동했음에도 다음 `4.요청`
에 동일하게 `POST` 방식 과 `메시지`를 유지 하는 것을 볼수 있다.


![Pasted image 20231231215206.png](https://file-api.ksq9511.synology.me:5353/obsidian-image/20241103181554.png)


# 일시 리다이렉션

리소스의 URL 이 일시적으로 변경되는 것을 의미 한다. 주문 완료후 주문 내역 화면으로 이동할때 주로 사용한다.


## 302 : Found

^153daf


[ 301](3xx%20%EB%B2%88%EB%8C%80%20http%20%EC%83%81%ED%83%9C%EC%BD%94%EB%93%9C.md#^1e460b%20) 과 비슷 하다 리다이렉트 요청시 메서드가 GET 으로 변하고 본문이 제거 된다.


## 303 : See Other

[ 302](3xx%20%EB%B2%88%EB%8C%80%20http%20%EC%83%81%ED%83%9C%EC%BD%94%EB%93%9C.md#^153daf%20) 와 동일하게 동작 한다.


## 307 : Temporary Redirect 

^4c4274

[ 302](3xx%20%EB%B2%88%EB%8C%80%20http%20%EC%83%81%ED%83%9C%EC%BD%94%EB%93%9C.md#^153daf%20) 와 기능은 같으며 리다이렉트 요청 시 메서드의 본문을 유지한다. 


# 특수 리다이렉션

결과 대신 캐시를 사용한다.

--- 

# 참고

https://www.inflearn.com/course/http-%EC%9B%B9-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC
