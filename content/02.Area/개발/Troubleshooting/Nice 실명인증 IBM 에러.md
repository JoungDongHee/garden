---
date: 2023 년 1 월 31 일 00 시 11 분
tags:
  - Dev
  - Nice인증
  - troubleshooting
author: Joung Dong Hee
share: true
---

# Nice 실명인증 IBM 에러


클라이언트에 측에서 Nice 인증을 통한 실명 인증 기능을 원하셨고 이를 위해 개발 까지 완료한뒤 실 서버 에 반영한뒤 테스트를 진행했으나 **Nice 인코딩 에서 IBM 관련 에러가 발생** 


> [!bug] Error
> java.lang.NoClassDefFoundError: com/ibm/crypto/provider/IBMJCE
> 	at com.primeedunet.cleon.nice.service.NiseService.callNice(NiseService.java:24) ~[classes!/:na]
> 	at com.primeedunet.cleon.nice.controller.NiceController.NiceEncdata(NiceController.java:31) ~[classes!/:na]
> 	at jdk.internal.reflect.GeneratedMethodAccessor509.invoke(Unknown Source) ~[na:na]
> 	at java.base/jdk.internal.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43) ~[na:na]
> 	at java.base/java.lang.reflect.Method.invoke(Method.java:566) ~[na:na]
> 	at org.springframework.web.method.support.InvocableHandlerMethod.doInvoke(InvocableHandlerMethod.java:205) ~[spring-web-5.3.22.jar!/:5.3.22]
> 	at org.springframework.web.method.support.InvocableHandlerMethod.invokeForRequest(InvocableHandlerMethod.java:150) ~[spring-web-5.3.22.jar!/:5.3.22]
> 	at org.springframework.web.servlet.mvc.method.annotation.ServletInvocableHandlerMethod.invokeAndHandle(ServletInvocableHandlerMethod.java:117) ~[spring-webmvc-5.3.22.jar!/:5.3.22]
> 	at org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter.invokeHandlerMethod(RequestMappingHandlerAdapter.java:895) ~[spring-webmvc-5.3.22.jar!/:5.3.22]
> 	at


## 해결 방법

자료를 계속 찾아봤으나 정보가 부족하여 결국 Nice 측 에 문의한 결과 Nice 인증 하기 위한 JAR 파일은 하나만 사용해야 한다고 전달 받음

실제로 Nice 에서 제공한 JAR 라이르러의 경우 **NiceID.jar , NiceID_ibm.jar** 으로 총 2개의 라이브러리 파일을 제공하였고 이를 같이 빌드 및 배포한 것이 문제가 됨

각 JAR 파일은 Vender 사에 맞게 사용을 해줘야 하며 ibm jdk 는 NiceID_ibm.jar , Sun oracle Jdk 는 NiceID.jar 사용해야함 다음과 같이 벤더 사를 확인한뒤 맞는 벤더에 맞춰서 라이브러리 를 사용해야함

```bash
java -XshowSettings:all -version
```

![image.png](https://file-api.ksq9511.synology.me:5353/obsidian-image/20241127001936.png)

