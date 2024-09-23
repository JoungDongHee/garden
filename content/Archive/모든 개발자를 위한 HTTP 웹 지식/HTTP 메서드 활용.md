---
create: 2023-12-30
---
## 클라이언트에서 서버로 데이터 전송
* 쿼리 파라미터를 통한 데이터 전송
	* GET
	* 주로 정렬 필터(검색어)
* 메시지 바디를 통한 데이터 전송
	* POST
	* PUT
	* PATCH
	* 회원가입 , 상품주문 , 리소스 등록 , 리소스 변경 
### 데이터 전송 4가지
* 정적 데이터 조회
	* 이미지, 정적 텍스트 같이 일반적으로 쿼리파라미터 없이 리소스 경로로 단순하게 조회 가
	* /statuc/star.jpg
	* ![[Pasted image 20231230205934.png]]
* 동적 데이터 조회
	* 주로 검색 , 게시판 목록에서 정렬필터 
	* 조건을 줄여주는 필터 , 조회 결과 를 정렬하는 정렬 조건에 사용
	* /serach?q=hello&hi=ko
	* 쿼리파라미터 를 기반으로 정렬 필터해서 결과를 동적으로 생성
	* ![[Pasted image 20231230210422.png]]
	* 
* HTML Form 을 통한 데이터 전송
	* GET , POST 만 지원
	* 회원가입 , 상품주문 , 데이터 변경
	* username=kim&age=20 과 같이 쿼리 파라미터 처럼 Body 에 넣어서 전
	* ![[Pasted image 20231230210532.png]]
	* GET 방식은 조회에만 사용해야 하며 리소스 변경이 발생하는 곳에서 사용하면 안
	* ![[Pasted image 20231230210917.png]]
	* 파일 전송
		* Content-Type : multipart/form-data
		* 파일 업로드 같은 바이러니 데이터 전송시 사용 
		* 다른 종류의 여러 파일과 같은 폼의 내용 함께 전송 가능 
		* ![[Pasted image 20231230211050.png]]
* HTTP API 를 통한 데이터 전송
	* ![[Pasted image 20231230211427.png]]
	* 회원가입 , 상품주문 , 데이터 변경
	* 서버 TO 서버 
		* 백엔드 시스템 통신
	* 앱 클라이언트 
		* 아이폰 , 안드로이드
	* 웹 클라이언트 (Ajax)
		* Form 전송 대신 자바 스크립트 통신을 통해 사용
		* React , Vue 같은 웹 클라이언트 와 API 통신
	* PUT, POST , Patch 메시지 바디를 통해 데이터 전송
	* Content-Type : application/json  을 주로 사용
		* Text , Xml , Json 등

## HTTP API 설계 예시
* HTTP API - 컬렉션
	* POST 기반 등록
		* 클라이언트는 등록될 리소스의 URL 를 모른다
		* 회원 등록 /members -> post
			* 서버가 새로 등록된 리소스의 URL 를 생성해준다 
			* 예 ) 회원 등록시 회원 ID 를 반환
				* Location : /members/100
	* 예) 회원 관리 API 제공
* HTTP API - 스토어
	* PUT 기반 등록
		* 파일목록 /files ->Get
		* 파일 조회 /files/{filename} -> Get
		* 파일 등록 /files/{filename} -> Put
		* 파일삭제 /files/{filename} - > Delete
		* 파일 대량 등록 /files/{filename} - >POST
	* Store
		* 클라이언트가 관리 하는 리소스 저장소
		* 클라이언트가 리소스의 URL 를 관리
		* 여기서 스토어는 /files
	* 예) 정적 컨텐츠 관리 , 원격 파일 관리
* HTML FORM 사용
	* 웹 페이지 회원 관리
		* 회원 목록 /members -> GET
		* 회원 등록 폼 /members/new -> GET
		* 회원 등록 /members/new , /members ->POST
		* 회원 조회 /members/{id} -> GET
		* 회원 수정 폼 /members/{id}/edit - >GET
		* 회원 수정 /members/{id}/edit , /members/{id} - >POST
		* 회원 삭제 /members/{id}/delete - >POST
	* GET , POST 만 지원 하기 때문에 제약을 해결하기 위해서는 **새로운 동사 리소스 경로를 사용 해야한다**
	* POST 의 /new , /edit , /delete 같이 동작하고자 하는 URI 를 넣어준다
	* HTTP 메서드 로 해결하기 애매한 경우에도 사용




## URI 설계 개념
* 참고 : https://restfulapi.net/resource-naming
	* 문서 (Document)
		* 단일 개념(파일하나,객체인스턴스 , 데이터베이스 Row)
		* 예) members/100 , /files/start.jpg
	* 컬렉션(Collecttion)
		* 서버가 관리하는 리소스 디렉터리
		* 서버가 리소스의 URI 를 생성하고 관리
		* 예) /members
	* 스토어(Store)
		* 클라이언트가 관리하는 자원 저장소
		* 클라이언트가 리소스의 URI 를 알고 관리
		* 예) /files
	* 컨트롤러(Controller) , 컨트롤 URI
		* 문서 , 컬렉션 , 스토어 해결하기 어려운 추가 프로세스 실행
		* 동사를 직접 사용
		* 예) /members/{id}/delete