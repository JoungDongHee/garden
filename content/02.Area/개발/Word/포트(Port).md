---
date: 2024 년 11 월 03 일 19 시 11 분
tags:
  - Dev
  - Port
  - Web
author: Joung Dong Hee
share: true
---

# 포트(Port)

동일한 IP 를 구분 하기 위해 사용하는 기술이다.

예를들어 Server 의 IP 가 `200.200.200.2` 일 경우 동일한 IP 에서 `게임` , `화상전화` , `REST API` 등 3개의 서비스를 동시에 제공하고 있다고 가정 할 경우 이를 구분짓기 위해서  `200.200.200.2:80` , `200.200.200.2:22` 와 같이  **:**  를 사용하여 프로세스를 구분한다. 

![image.png](https://file-api.ksq9511.synology.me:5353/obsidian-image/20241103200314.png)



## 대표 Port 번호

* FTP : 20 , 21
* SSH : 22
* TELNET : 23
* HTTP : 80
* HTTPS : 443

--- 

# 참고


https://ittrue.tistory.com/185

https://www.inflearn.com/course/http-%EC%9B%B9-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC