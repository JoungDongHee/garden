---
create: 2024-01-01
---
## HTTP 헤더용도
* HTTP 전송에 필요한 모든 부가정보
* 예) 메시지 바디의 내용 , 메시지 바디의 크기, 압축 , 인증 , 요청 클라리언트 , 서버 정보 , 캐시 정보 , 관리 정보 등등
* 표준 헤더가 너무 많음
* 필요시 임의의 헤더 추가 가능
## RFC2616
* ![[Pasted image 20240101173008.png]]
* ![[Pasted image 20240101173105.png]]
## RFC723x
* ![[Pasted image 20240101173322.png]]
* Entity -> 표현 (Representaion)
* 표현 = 표현 메타데이터 + 표현데이터
## 표현
	![[Pasted image 20240101173750.png]]
* Content-Type : 표현데이터의 형식 설명
	* 미디어타입 , 문자 인코딩
	* 예)
		* text/html; charset=utf-8
		* application/json
		* image/png
* Content-Enconding: 표현데이터의 압축방식
	* 표현데이터를 압축하기 위해 사용
	* 데이터를 전달하는 곳에서 압축 후 인코딩 헤더 추가
	* 데이터를 읽는 쪽에서 인코딩 헤더의 정보로 압축해제
	* 예)
		* gzip
		* deflate
		* identity -> 압축 안
* Content-Language : 표현 데이터의 자연언어
	* 표현데이터의 자연 언어
	* 표현 데이터의 자연 언어를 표현
	* 예)
		* ko
		* en
		* en-US
* Content-Length : 표현데이터의 길이
	* 바이트 단위
	* Transfer-Encoding(전송코딩) 을 사용하면 Content-Length 를 사용하면 안
* 표현 헤더는 전송 , 응답 둘다 사용
## 협상(콘텐츠 네고시에이션)
* 클라이언트가 선호하는 표현 요청
* Accept : 클라이언트가 선호하는 미디어 타입 전달
* Accept-Charset : 클라이언트가 선호하는 문자 인코딩
* Accept-Encoding : 클라이언트가 선호하는 압축 인코딩
* Accept-Language : 클라리언트가 선호하는 자연언어
* 협상 헤더는 요청시에만 사용
* 예)
	* ![[Pasted image 20240101174538.png]]
	* ![[Pasted image 20240101174625.png]]
		* 클라이언트에서 요청시 Accept-Language : ko 같이 원하는 언어를 서버에 요청하면 서버는 해당 요청에 맞춰서 한국어로 반환을 해준다
	* ![[Pasted image 20240101174747.png]]
### 협상과 우선순위 1
* ![[Pasted image 20240101175136.png]]
* ![[Pasted image 20240101175235.png]]
* Quality Values(q) 값 사용
* 0~1 클수록 높은 우선 순위
* 생략하면 1
* Accept-Language : ko-KR,ko:q0.9,en-US;0.8,en;q=0.7
	* 1. ko-Kr;q=1(q생략)
	* ko;q=0.9
	* en-Us;q=0.8
	* en;q=0.7
### 협상과 우선 순위2
* 구체적인 것이 우선한다.
* Accept: text/* , text/plain, text/plain;format=flowed,* / * 
	* text/plain;format=flowed
	* text/plain
	* text/*
	* * / *
### 협상과 우선순위 3
* 구체적인 것을 기준으로 미디어 타입을 맞춘다
* Accept:text/* ;q=0.3, text/html;q=0.7,text/html;level=1,text/html;level=2;q=0.4,* / * ; q=0.5
	* text/html;level=1
	* text/html;q=0.7
	* text/plain=0.3 -> 위 요청값에서 매칭 되는게 없기 때문에 text/* 값의 q(퀄리티) 값인 0.3 을 따라간
	* image/jpeg=0.5
	* text/html;level=2
	* text/html;level=3
## 전송방식
* 단순전송
	* ![[Pasted image 20240101180047.png]]
* 압축전송 
	* ![[Pasted image 20240101180149.png]]
	* Content-Encoding 을 반드시 추가해야함
* 분할전송
	* ![[Pasted image 20240101180231.png]]
* 범위전송
	* ![[Pasted image 20240101180450.png]]
## 일반정보
### From
* 유저 에이전트의 이메일 정보
* 일반적으로 잘 사용되지 않음
* 검색엔진 같은 곳에서 주로 사용
* 요청에서 사용
### Referer
	![[Pasted image 20240101212655.png]]
* 이전 웹페이지 주소
* 현재 요청된 페이지 이전의 웹 페이지 주소
* A->B 로 이동하는 경우 B를 요청할때 Referer:A 를 포함해서 요청
* Referer 를 사용해서 유입경로 분석 가능
* 요청에서 사용
### User Agent
	![[Pasted image 20240101212924.png]]
* 클라이언트의 애플리케이션 정보(웹 브라우저 정보 등)
* 통계 정보
* 어떤 종류의 브라우저에서 장애가 발생하는지 파악 가능
* 요청에서 사용
### Server
* 요청을 처리하는 ORIGIN 서버의 소프트웨어 정보
* Server:Apache/2.2.22(Debian)
* server: nginx
* 응답에서 사용
### Date
* 메시지가 발생한 날자와 시간
* 응답에서 사
## 특별한 정보
### HOST 
	![[Pasted image 20240101213443.png]]
	* 하나의 IP 주소에서 여러개의 도메인을 가진 애플리케이션이 구동 중 일때 헤더의 Host 정보를 확인하여 해당 애플리케이션에 요청을 한다
* 요청한 호스트 정보(도메인)
* 요청에서 사용 
* **필수**
* 하나의 서버가 여러도메인을 처리해야 할때
* 하나의 IP 주소에 여러 도메인이 적용되어 있을때
### Location
* 웹 브라우저는 3xx 응답의 결과에 Location 헤더가 있으면 Location 위치로 자동 이동(리다이렉트)
* 응답코드 3xx 에서 설명
* 201(Created) : Location 값은 요청에 의해 생성된 리소스 URI
* 3xx(Redirection): Location 값은 요청을 자동으로 리디렉션 하기 위한 대상 리소스를 가리킴킴
### Allow
* 허용 가능한 Http 메서드
* 405 (Method Not Allowed) 에서 응답에 포함해야함
* Allow: Get , Head , Put
### Retry-After
* 유저 에이전트가 다음 요청을 하기 까지 기다려야 하는 시간
* 503(Service Unavailable) : 서비스가 언제까지 불능인지 알려줄수 있음
* ![[Pasted image 20240101213947.png]]

## 인증
* Authorization : 클라이언트 인증 정보를 서버에 전달
	* Basic xxxxxxxxxxxxxx
* WWW-Authenticate : 리소스 접근시 필요한 인증 방법 정의
	* 401  Unauthorized 응답과 함께 사용


## 쿠키
* Set-Cokkie : 서버에서 클라이언트로 쿠키전달(응답)
* Cookie : 클라이언트가 서버에서 받은 쿠키를 저장하고 , Http 요청시 서버로 전달
* 예)set-cokie: **sessionid=abcde1234**;expires=Sat 26-Dec-2020 00:00:00 GMT;**path**/; **domain**=google.com;**Secure**
* 사용처 
	* 사용자 로그인 세션 관리
	* 광고 정보 트래킹
* 쿠키 정보는 항상 서버에 전송됨
	* 네트워크 트래픽 추가 유발
	* 최소한의 정보만 사용(세션id, 인증토큰) -> **사용자 정보를 담지 말것**
	* 서버에 저장하지 않고 웹 브라우저 내부에 데이터를 저장하고 싶으면 웹 스토리지 사용
* 주의 !
	* 보안에 민감한 데이터는 저장하면 안됨(주민번호 , 신용카드 번호 등)
### 쿠키 미사용
	![[Pasted image 20240101214411.png]]
	![[Pasted image 20240101214512.png]]
	![[Pasted image 20240101214622.png]]
	![[Pasted image 20240101214636.png]]
* 모든 요청에 사용자 정보가 포함되도록 개발 해야함
* 브라우저를 완전히 종료하거 다시 열면?

### 쿠키 사용
	![[Pasted image 20240101214806.png]]
	![[Pasted image 20240101214832.png]]
	![[Pasted image 20240101214858.png]]
### 생명주기
* expires=Sat 26-Dec-2020 00:00:00
	* 해당 기간 만료일이 되면 쿠키 삭제
* max-age=3600(3600 초)
	* 0 이나 음수를 지정하면 쿠키 삭제
* 세션 쿠키: 만료 날짜를 생략하면 브라우저 종료 시 까지만 유지
* 영속 쿠키: 만료날짜를 입력하면 해당 날짜 까지 유지
### 도메인
* 명시 : 명시한 문서 기준 도메인 + 서브도메인 포함
	* domain=example.org 를 지정해 쿠키를 생성
		* example.org 를 포함한 해당 dev.example.org , api.example.org  같이 해당 도메인을 사용하는 모든 서브 도메인에 쿠키 접근
	* 생략
		* example.org 에서 쿠키를 생성하고 domain 지정을 생략 할경우
			* example.org 에서만 접근 가능 dev.example.org , api.example.org 접근 불가
### 경로 
* path=/home
* 이 경로를 포함한 하위 경로 페이지만 쿠키 접근
* 일반정으로 path=/ 루트로 지정
* 예)
	* path/home 지정
		* /home -> 접근 가능
		* /home/level1 ->가능
		* /home/level1/level2 -> 가능
		* /hello -> 불가
### 보안
* Secure 
	* 쿠키는 Http , Https 를 구분하지 않고 전송
	* Secure 를 적용하면 https 인 경우에만 전송
* HttpOnly
	* XSS 공격방지
		* ![[Pasted image 20240101221221.png]]
		* 공격자가 웹 페이지에 악의적인 스크립트를 삽입하여, 해당 스크립트가 웹 페이지를 열람하는 사용자들에게 실행되도록 하는 공격입니다.
		* 사용자가 \<script>alert('XSS')\</script> 같이  스크립트를 사용할경우 클라이언트에서 해당 내용을 실행할수 있다
	* 자바스크립에서 접근 불가
	* HTTP 전송에만 사용
* SameSite
	* XSRF 공격 방지
		* **인가된 사용자의 신뢰를 이용:** 사용자가 웹 애플리케이션에 로그인한 상태에서 악의적인 웹 페이지를 방문하면, 해당 페이지에서는 사용자의 권한을 이용하여 웹 애플리케이션에 자동으로 요청을 보낼 수 있습니다.
		* **인가된 사용자의 브라우저를 이용:** 악의적인 스크립트나 이미지 등이 포함된 웹 페이지를 희생자의 브라우저에서 실행될 때, 이 스크립트가 희생자의 권한으로 웹 애플리케이션에 요청을 보낼 수 있습니다.
	* 요청 도메인과 쿠키에 설정된 도메인이 같은 경우에만 쿠키 전