---
date: 2024 년 11 월 05 일 23 시 11 분
tags:
  - Dev
  - WebApplicatoinServer
  - WAS
author: Joung Dong Hee
share: true
---

# WAS(Web Applicatoin Server)


WAS 는 [WS(Web Server)](WS(Web%20Server).md) 와 동일하게 [HTTP (Hyper Text Transfer Protocol )](HTTP%20(Hyper%20Text%20Transfer%20Protocol%20).md) 를 기반으로 동작 한다. 웹 서버에서 하던 기능을 WAS 에서 또한 동일하게 수행하며 사용자에게 [동적 웹 페이지(Dynamic Web Page)](%EB%8F%99%EC%A0%81%20%EC%9B%B9%20%ED%8E%98%EC%9D%B4%EC%A7%80(Dynamic%20Web%20Page).md) 를 전달할수 있다.

대표적인 WAS 제품으로는 Tomcat, JBoss, Jetty, WebSphere 등이 있다.


![image.png](https://file-api.ksq9511.synology.me:5353/obsidian-image/20241112234716.png)


## WAS 와 WS 를 같이 사용하는 이유 


대부분의 많은 아키텍처 들 이 제일 앞에 Nginx 와 같은 [WS(Web Server)](WS(Web%20Server).md) 를 두고 뒤에 는 Tomcat [WAS(Web Applicatoin Server)](WAS(Web%20Applicatoin%20Server).md) 를 두는 형태의 아키텍처를 사용한다.  

이런 구조를 사용함으로서 얻을수 있는 이점은 다음과 같다.

1. **역할 분리**: 웹 서버와 WAS의 역할을 명확하게 분리함으로써 각자의 기능을 최적화할 수 있습니다. 웹 서버는 정적 파일(이미지, HTML, CSS, JS 등)을 효율적으로 처리하고, WAS는 동적 요청 처리에 집중할 수 있습니다.
2. **로드 밸런싱 및 프록시**: Nginx와 같은 웹 서버는 로드 밸런싱, 프록시 등의 기능을 수행하여 트래픽을 효과적으로 관리할 수 있습니다.
3. **확장성**: 웹 서버와 WAS를 분리하면 각 컴포넌트를 독립적으로 확장할 수 있어 시스템 확장성이 높아집니다.

### 추가

* Spring Boot 는 WAS 의 기능을 내장한 프레임 워크 이다. Spring boot 는 특별한 설정을 하지 않으면 내장된 Tomcat 위에 구동을 하며 WAS 처럼 Http 요청을 처리할수 있다.


---

# 참고

https://yozm.wishket.com/magazine/detail/1780/

https://gmlwjd9405.github.io/2018/10/27/webserver-vs-was