---
date: 2024 년 11 월 03 일 18 시 11 분
tags:
  - Dev
  - Http
  - Redirection
author: Joung Dong Hee
share: true
---
# 리다이렉션

실제 리소스 주소 , 폼 혹은 웹 애플리케이션이 다른 URL 에 위치하고 있을 경우 다른 곳 으로 이동 하도록 하는 기술이다.

## 리다이렉션 예시

1. `GET /doc HTTP/1.1` 로 클라이언트가 Server 에 요청을 보낸다.
2. `GET /doc HTTP/1.1` URL 은 더이상 사용하지 않기 때문에 SEVER 에서는 응답 본문 location 에 새로운 URL `/doc_new` 을 담아서 전달한다.
3. `/doc_new` 를 전달받은 클라이언트 해당 URL 로 다시 이동 혹은 요청을 Server 에 보내게 된다.
4. 전달받은 URL 에 맞게 Server 에서는 응답을 해준다

![image.png](https://file-api.ksq9511.synology.me:5353/obsidian-image/20241103184147.png)


---

# 참고

https://developer.mozilla.org/ko/docs/Web/HTTP/Redirections

https://www.inflearn.com/course/http-%EC%9B%B9-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC