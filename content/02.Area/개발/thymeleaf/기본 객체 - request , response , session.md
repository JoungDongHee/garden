---
date: 2025 년 03 월 18 일 12 시 03 분
tags:
  - Dev
  - Thymeleaf
  - request
  - response
  - session
author: Joung Dong Hee
share: true
---

# 기본 객체 - request , response , session

Thymeleaf에서는 Spring이 제공하는 기본 객체들에 쉽게 접근할 수 있습니다. 이를 통해 `request`, `response`, `session`, `servletContext` 등을 활용할 수 있습니다.

## 1. 기본 객체 접근 방법

Spring 컨트롤러에서 다음과 같이 데이터를 설정할 수 있습니다.

```java
session.setAttribute("sessionData", "Hello Session");  
model.addAttribute("request", request);  
model.addAttribute("response", response);  
model.addAttribute("servletContext", request.getServletContext());  

```

Thymeleaf에서 이를 출력하는 방법은 다음과 같습니다.


```html
<ul>
    <li>request = <span th:text="${request}"></span></li>
    <li>response = <span th:text="${response}"></span></li>  
    <li>session = <span th:text="${session}"></span></li>  
    <li>servletContext = <span th:text="${servletContext}"></span></li>  
    <li>locale = <span th:text="${#locale}"></span></li>
</ul>

```

이렇게 렌더링할 경우, 객체의 실제 값이 아니라 주소값이 출력됩니다.

```html
<ul>
    <li>request = <span>org.apache.catalina.connector.RequestFacade@18b48246</span></li> 
    <li>response = <span>org.springframework.web.context.request.async.StandardServletAsyncWebRequest$LifecycleHttpServletResponse@2da281fe</span></li>
    <li>session = <span>org.thymeleaf.context.WebEngineContext$SessionAttributeMap@3eefa686</span></li>
    <li>servletContext = <span>org.apache.catalina.core.ApplicationContextFacade@a27b9cd</span></li>
    <li>locale = <span>ko_KR</span></li>
</ul>

```


## 2. Thymeleaf 제공 기본 객체

Thymeleaf는 기본적으로 몇 가지 편의성 객체를 제공합니다.

### 2.1 `param` - 요청 파라미터 값

요청에서 전달된 파라미터 값을 출력할 수 있습니다.

```html
<li>Request Parameter = <span th:text="${param.paramData}"></span></li>
```

예를 들어 `http://localhost:8080/test?paramData=Hello` 로 요청하면 다음과 같이 출력됩니다.

```html
<li>Request Parameter = <span>Hello</span></li>
```

### 2.2 `session` - 세션 데이터 접근

`session` 객체를 사용하여 `session.setAttribute("key", value)` 로 설정한 값을 Thymeleaf에서 직접 사용할 수 있습니다.

```html
<li>session = <span th:text="${session.sessionData}"></span></li>
```

### 2.3 `@beanName` - Spring Bean 사용

`helloBean`이라는 이름의 Spring Bean이 컨테이너에 등록되어 있을경우
```java
@Component("helloBean")
public class HelloBean {
    public String hello(String name) {
        return "Hello, " + name;
    }
}
```

Spring에서 등록된 Bean을 Thymeleaf에서 사용할 수 있습니다.

```html
<li>spring bean = <span th:text="${@helloBean.hello('Spring!')}"></span></li>
```

