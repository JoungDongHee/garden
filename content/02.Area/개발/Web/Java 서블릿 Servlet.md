---
date: 2025 년 02 월 10 일 01 시 02 분
tags:
  - Dev
  - Web
  - Servlet
  - CGI
author: Joung Dong Hee
share: true
---

# Java 서블릿(Servlet)


서블릿은 **자바 기반의 웹 애플리케이션 프로그래밍 기술**로, 동적으로 웹 페이지를 생성하기 위해 사용됩니다.  
서블릿은 웹 서버와 클라이언트 간의 요청과 응답을 처리하는 역할을 하며, 흔히 **"자바 기반의 CGI(Common Gateway Interface)"** 라고도 합니다.  
하지만 CGI는 요청마다 새로운 프로세스를 생성하는 방식이므로 성능상 단점이 있습니다. 반면 서블릿은 **멀티스레드 기반으로 동작**하여 요청을 효율적으로 처리할 수 있습니다.

## 서블릿의 역할

서블릿은 클라이언트와 서버 간의 HTTP 요청을 처리하는 **"문지기"** 역할을 수행합니다.  
다음은 클라이언트가 `POST` 방식으로 데이터를 전송할 때 HTTP 요청의 예시입니다.

```txt
POST /submit-form HTTP/1.1
Host: www.example.com
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/90.0.4430.93 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Content-Type: application/x-www-form-urlencoded
Content-Length: 27
Connection: keep-alive
Upgrade-Insecure-Requests: 1

username=test&password=1234
```


서블릿은 이러한 **HTTP 요청을 처리하고, 적절한 응답을 생성하는 역할**을 합니다.  
또한, HTTP 요청과 응답을 캡슐화하는 객체인 `HttpServletRequest`, `HttpServletResponse`를 제공합니다.

## HttpServletRequest

`HttpServletRequest` 객체는 **클라이언트가 보낸 요청 정보를 처리하는 역할**을 합니다.  
이 객체를 통해 요청된 HTTP 메서드, 요청 URL, 요청 파라미터 등을 확인할 수 있습니다.

다음은 `HttpServletRequest` 객체의 일부 메서드 입니다.

```java
String getMethod();  // HTTP 메서드(GET, POST 등) 반환
  
String getPathInfo();  // 추가적인 경로 정보 반환
  
String getContextPath();  // 컨텍스트 경로 반환
  
String getQueryString();  // 요청된 쿼리 스트링 반환
  
String getRequestURI();  // 전체 요청 URI 반환
  
StringBuffer getRequestURL();  // 전체 요청 URL 반환
  
HttpSession getSession();  // 현재 요청의 세션 반환
```


## HttpServletResponse

`HttpServletResponse` 객체는 **서블릿이 클라이언트에게 응답을 보낼 때 사용하는 객체**입니다.  
이 객체를 통해 [HTTP 상태코드](Http%20%EC%9D%98%20%EC%83%81%ED%83%9C%20%EC%BD%94%EB%93%9C.md), 응답 헤더 설정, 데이터 전송 등의 작업을 수행할 수 있습니다.

```java
int SC_CONTINUE = 100;  
int SC_SWITCHING_PROTOCOLS = 101;  
int SC_OK = 200;  
int SC_CREATED = 201;  
int SC_ACCEPTED = 202;  
int SC_NON_AUTHORITATIVE_INFORMATION = 203;  
int SC_NO_CONTENT = 204;  
int SC_RESET_CONTENT = 205;  
int SC_PARTIAL_CONTENT = 206;  
int SC_MULTIPLE_CHOICES = 300;  
int SC_MOVED_PERMANENTLY = 301;  
int SC_MOVED_TEMPORARILY = 302;  
int SC_FOUND = 302;  
int SC_SEE_OTHER = 303;  
int SC_NOT_MODIFIED = 304;  
int SC_USE_PROXY = 305;  
int SC_TEMPORARY_REDIRECT = 307;  
int SC_BAD_REQUEST = 400;  
int SC_UNAUTHORIZED = 401;  
```

# Servlet 동작 원리

서블릿은 다음과 같은 원리로 동작


![image.png](https://file-api.ksq9511.synology.me:5353/obsidian-image/20250211233396.png)


- **클라이언트가 HTTP 요청을 보냄** (Request)
- **웹 서버가 요청을 웹 컨테이너에 전달**
- **웹 컨테이너가 `HttpServletRequest` 객체를 생성하여 서블릿에 전달**
- **서블릿이 요청을 처리하고 `HttpServletResponse` 객체를 생성**
- **웹 컨테이너가 응답을 웹 서버에 전달**
- **웹 서버가 최종적으로 클라이언트에게 응답 전송**

> [!info] 웹 컨테이너
> 웹 컨테이너는 서블릿을 실행하고 요청을 서블릿에 전달하는 역할을 하는 소프트웨어입니다.  
> 대표적인 웹 컨테이너로는 **Apache Tomcat, Jetty, JBoss** 등이 있습니다.  
> 웹 컨테이너는 서블릿의 생명주기를 관리하고, 멀티스레드 기반으로 동작하여 다수의 요청을 효율적으로 처리합니다.


# Servlet Life Cycle

서블릿의 생명주기는 다음과 같은 메서드로 관리됩니다.

```java
public class HttpServlet {  
    public void init(ServletConfig config) throws ServletException {  
        // 서블릿 초기화, 최초 1회 실행됨  
    }  

    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
        // 클라이언트 요청을 처리하고 응답 생성  
    }  
  
    public void destroy() {  
        // 서블릿 종료 시 실행됨  
    }  
}

```

### 서블릿 생명주기의 단계별 설명

1. **init()**
    - 서블릿이 최초 생성될 때 한 번만 호출됩니다.
    - 초기화 작업(예: 데이터베이스 연결 설정 등)에 사용됩니다.
2. **service()**
    - 클라이언트의 요청이 들어올 때마다 호출됩니다.
    - 요청된 HTTP 메서드(GET, POST 등)에 따라 `doGet()`, `doPost()` 등의 메서드가 호출됩니다.
    - 멀티스레드 환경에서 동작합니다.
3. **destroy()**
    - 서블릿이 종료될 때 한 번만 호출됩니다.
    - 자원 해제(예: 데이터베이스 연결 닫기, 쓰레드 종료 등)에 사용됩니다.


# 서블릿의 싱글톤 패턴

서블릿은 **기본적으로 [싱글톤 패턴(Singleton Pattern)](%EC%8B%B1%EA%B8%80%ED%86%A4%20%ED%8C%A8%ED%84%B4(Singleton%20Pattern).md)으로 관리됩니다.**  즉, 하나의 서블릿 인스턴스가 생성되면 여러 요청이 해당 인스턴스를 공유하며 멀티스레드 방식으로 동작합니다.

하지만, **모든 요청에 대해 하나의 인스턴스를 공유하는 것은 아닙니다.**  
서블릿은 URL 매핑에 따라 개별적으로 인스턴스가 생성될 수 있습니다.

```java
@WebServlet(name = "singleton1", urlPatterns = "/singleton1")  
public class TestServlet1 extends HttpServlet {  
    @Override  
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
        // 비즈니스 로직 처리  
    }  
}

```

```java
@WebServlet(name = "singleton2", urlPatterns = "/singleton2")  
public class TestServlet2 extends HttpServlet {  
    @Override  
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
        // 다른 비즈니스 로직 처리  
    }  
}

```

위 예제에서 `/singleton1`과 `/singleton2`는 각각 다른 서블릿 인스턴스로 관리됩니다.


> [!warning] 주의할 점
> 싱글톤 서블릿이므로 인스턴스 변수(멤버 변수)를 사용할 때 주의해야 합니다.
> 여러 요청이 동시에 같은 서블릿을 공유하므로, 멤버 변수에 상태를 저장하면 데이터 충돌이 발생할 수 있습니다.
> 따라서, 서블릿 내부에서 상태를 유지해야 할 경우 HttpSession이나 ThreadLocal을 활용하는 것이 안전합니다.
> 
