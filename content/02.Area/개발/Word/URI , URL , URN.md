---
date: 2024 년 11 월 03 일 19 시 11 분
tags:
  - Dev
  - URL
  - URI
  - URN
author: Joung Dong Hee
share: true
---

# URI(Uniform-Resource-Identifier)

`URL` 과 `URN`  을 통틀어 `URI` 라고 하며 하나의 리소스를 가르키는 문자열을 의미한다.

![Pasted image 20231226214902.png](https://file-api.ksq9511.synology.me:5353/obsidian-image/20241103193936.png)


# URL(Uniform-Resource-Locator)

^c13188

인터넷 에서 웹페이지 , 이미지 , 비디오 등 리소스의 위치를 가르키는 문자열을 의미하며 
브라우저에서는 https://developer.mozilla.org 와 같이 URL 주소를 창에 표시한다. 

# URN(Uniform-Resource-Name)

위치나 존재 여부를 지정하지 않고 리소스를 참조 하는 표준 형식의 URI 입니다.


![Pasted image 20231226215007.png](https://file-api.ksq9511.synology.me:5353/obsidian-image/20241103193747.png)


## URL 의 기본 구조

![image.png](https://file-api.ksq9511.synology.me:5353/obsidian-image/20241103194283.png)

* http : 프로토콜(Protocol) 을 의미 하여 어떤 방식으로 자원에 접근할지에 대한 방식 
	* https , ftp , ssh 등 
* www.example.com :  [ DNS](What%20is%20DNS.md) 를 의미한다. 
* 80 : [포트(Port)](%ED%8F%AC%ED%8A%B8(Port).md) 번호를 의미한다. 기본적으로 생력이 가능하다.
	* http : 80
	* https : 443
	* ssh : 22
* /path/to/myfile.html : Path 리소스의 경로나 위치등을 가르킨다.
* ?key1=value1&value2=value2 : parameter 로 웹서버 에 제공하는 값 을 의미한다.
* \#SomewherelknTheDocument : 서버에 전송하는 용도가 아닌 탭 에 대한 정보를 저장하거나 하는 용도
