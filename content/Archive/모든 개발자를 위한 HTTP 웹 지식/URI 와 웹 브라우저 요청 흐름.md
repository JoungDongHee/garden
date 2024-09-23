---
create: 2023-12-26
tags:
  - 습관
  - 공부
  - TCP
  - ip
  - URL
  - URI
  - URN
---
## URI 
* URI 는 로케이터(Locator) , 이름(name) 또는 둘다 추가로 분류될수 있
* ![[Pasted image 20231226214902.png]]
* ![[Pasted image 20231226215007.png]]
* 기본적으로 URL 를 사용한다
* URI
	* Uniform : 리소스를 식별하는 통일된 방식
	* Resource : 자원 , URI 로 식별할수 있는 모든 것 (제한 없음)
	* Identifier : 다른 항목과 구분하는데 필요한 정보 
* URL - Locator : 리소스가 있는 위치를 지정
* URN - Name : 리소스에 이름을 부여 
* 위치는 변할수 있지만 이름은 변하지 않는다
*  urn:isbn:896077331 (어떤 책의 isbn URN)
* URN 이름만으로 실제 리소스를 찾을수 있는 방법이 보편화 되지 않음

## 전체문법
* scheme://[userinfo@]host[[:port]][[/path]][[?query]][[#frament]]
* http://www.google.com:443/search?q=hello&hi=ko
	* 프로토콜(http)
		* 어떤 방식으로 자원에 접근할지에 대한 약속 혹은 규칙 
		* http,https ,ftp
	* 호스트(www.google.com)
		* 도메인명 혹은 IP 주소
	* 포트번호(443)
		* 접속포트 
		* 일반적으로 생략 가능 
		* 기본 -> http 80 , https 443
	* 패스(/search)
		* 리소스 경로 ,계층적 구조
		* 예시
			* /home/file.jpg
			* /members
			* /members/100/items/iphone12
	* 쿼리파라미터(q=hello&hi=ko)
		* key=value 형태
		* ? 시작 & 로 추가 가능 ?key=value&keyB=value
		* query parameter , query string 등 으로 불림 웹 서버에 제공하는 파라메터 형태 
	* frament
		* https://spring.io/projects/spring-webflow/#samples
		* ![[Pasted image 20231226220424.png]]
		* 위 이미지 처럼 특정 탭에 대한 정보를 저장할수 있
		* html 내부 북마크 등에 사용 
		* 서버에 전송하는 정보는 아님 

## 웹 브라우저 요청과 흐름 

1. 사용자가 URL 에 다음과 같이 입력할경우 해당 요청에 대해 HTTP 요청 메시지를 생성한다
![[Pasted image 20231226220610.png]]
2. 입력한 요청 메시지는 TCP/IP 패킷 으로 감싼뒤 요청 하고자 하는 서버에 전달한다.
![[Pasted image 20231226220557.png]]
![[Pasted image 20231226220737.png]]
3. 요청 받은 서버는 TCP/IP 으로 감싸진 패킷을 버리고 HTTP 메시지만 참조한뒤 해당 내용에 대해 응답을 준
![[Pasted image 20231226222012.png]]
