---
date: 2024 년 11 월 12 일 23 시 11 분
tags:
  - Dev
  - WS
  - WebServer
author: Joung Dong Hee
share: true
---

# WS(Web Server)

흔히 웹 서버 와 햇갈리는 것이 [WAS(Web Applicatoin Server)](WAS(Web%20Applicatoin%20Server).md) 인데 둘은 엄연히 다른 개념이다. 

웹 서버는 클라이언트 로부터 [HTTP (Hyper Text Transfer Protocol )](HTTP%20(Hyper%20Text%20Transfer%20Protocol%20).md) 요청을 받아들이고 해당 요청에 대한 처리를 수행하여 클라이언트에게 HTML 문서와 같은 [정적 웹 페이지(Static Web Page)](%EC%A0%95%EC%A0%81%20%EC%9B%B9%20%ED%8E%98%EC%9D%B4%EC%A7%80(Static%20Web%20Page).md) 를 전달해주는 역할을 한다.

대표적인 웹 서버 제품으로는 Nginx, Apache, IIS 등이 있다.


![제목 없는 다이어그램.drawio.png](https://file-api.ksq9511.synology.me:5353/obsidian-image/20241112233848.png)



웹 서버와 WAS의 주요 차이점은 다음과 같습니다:

1. **처리 대상**: 웹 서버는 정적 웹 페이지 처리에 특화되어 있는 반면, WAS는 동적 콘텐츠 생성에 특화되어 있습니다.
2. **기능**: 웹 서버는 HTTP 요청 처리, 로드 밸런싱, 캐싱 등의 기능을 담당하며, WAS는 비즈니스 로직 처리, DB 연동, 세션 관리 등의 기능을 담당합니다.
3. **성능**: 웹 서버는 정적 콘텐츠 처리에 최적화되어 있어 WAS보다 더 빠른 응답 속도를 보일 수 있습니다.

----

# 참고

https://gmlwjd9405.github.io/2018/10/27/webserver-vs-was
