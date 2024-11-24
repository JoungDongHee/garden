---
date: 2024 년 11 월 20 일 22 시 11 분
tags:
  - Dev
  - Java
  - JVM
author: Joung Dong Hee
share: true
---

# JVM(Java Virtual Machine)

자바 의 핵심 기능 중 하나이다. 
자바는 하나의 OS 에 종속되지 않는 특징이 있다. 예를들어 C 언어를 통해 프로그래밍을 한다고 가정할때 C 로 만들어진 프로그램을 실행 하기위해 `기계어` 로 컴파일 과정을 거친후 이를 실행한다. 
그리고 이 기계어로 변환하는 과정에서 OS (윈도우 , Mac, 리눅스 )는 서로 다른 기계어 를 사용한다.  그렇기 때문에 윈도우 에 맞춰서 컴파일을 진행한 프로그램을 Mac 에서는 동작을 하지 못한다. 


하지만 JVM 은 이러한 문제를 해결할수 있다. 바로 OS 바로 위에서 실행하는 것이 아닌 JVM 위에서 프로그램이 실행되기 때문에 OS 가 인식할수 있는 기계어가 아닌 JVM 이 인식할수 있는  Java bytecode(`*.class`)로 변환된다.


## JVM 구조 

JVM 은 다음과 같은 과정을 통해서 우리가 만든 자바 애플리케이션을 실행한다.

**클래스 로더** 가 컴파일된 자바 바이트 코드를 런타임 데이터 영역에 로드 하고 실행엔진이 자바의 자바이트 코드를 실행한다.

![image.png](https://file-api.ksq9511.synology.me:5353/obsidian-image/20241120230100.png)


--- 

# 참고

https://d2.naver.com/helloworld/1230
