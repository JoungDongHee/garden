---
date: 2024 년 11 월 03 일 20 시 11 분
tags:
  - Dev
  - 예외처리
  - CheckedException
  - 체크예외
author: Joung Dong Hee
share: true
---

# 체크 예외 (Checked Exception)

## Checked Exception 

Checked Exception은 컴파일 시점에 [예외처리](%EC%98%88%EC%99%B8%EC%B2%98%EB%A6%AC.md) 를 확인하는 예외로, `Exception` 클래스를 상속받되 `RuntimeException`을 상속하지 않는 예외들을 말합니다.
fasdfa
정반대로 [언체크 예외(UnChecked Excption)](%EC%96%B8%EC%B2%B4%ED%81%AC%20%EC%98%88%EC%99%B8(UnChecked%20Excption).md) 가 존재한다.

### 주요 특징

- 컴파일러가 예외 처리 여부를 강제 검사
- 반드시 try-catch로 처리하거나 throws로 선언 필요
- 주로 복구 가능한 예외 상황에서 사용
- 대표적인 예: `IOException`, `SQLException`, `ClassNotFoundException`

만약 다음과 같이 Checked 예외가 발생하는 코드 일 경우 `catch` 를 사용하지 않을 경우 `IDE` 혹은 `자바의 컴파일 단계에서 에러`가 발생하여 프로그램 의 실행이 불가능하다. 

```java
/**  
 * 예외를 잡아서 저리하는 코드 
 * 오류 발생
 * */
public void callCatch(){  
    try {  
        client.call();  
    }  
    System.out.println("정상 흐름");  
}
```


> [!danger] Error
> C:\Users\Administrator\Desktop\java-mid\src\exception\basic\checked\Service.java:10:9
> java: 'try' without 'catch', 'finally' or resource declarations


## Checked Exception 구현

### 사용자 정의 Checked Exception

`MyCheckedException` 은 `Exception` 을 상속 받아 구현한다.


```java
public class MyCheckedException extends Exception {  
    public MyCheckedException(String message){  
        super(message);  
    }  
}

```

###  예외 발생 클래스 (Client)

`Client` 클래스에서는 `call()` 메소드에서 에러가 발생시 `MyCheckedException` 클래스의 `MyCheckedException()` 메소드로 예외를 **강제**로 던지게 된다.

```java
public class Client {  
    public void call() throws MyCheckedException{  
        // 문제 발생  
        throw new MyCheckedException("ex");  
    }  
}
```

### 서비스 계층 구현

`Service` 클래스 에서는  `callCatch` 메소드 에서  `client.call()`  함수를 불러 실행을 하였다.  client.call() 함수 에서는 **MyCheckedException** 예외가 발생할수 있다. 또한 MyCheckedException 예외는 Exception 을 상속받아 사용하는 **체크예외** 이기 때문에 다음과 같이  catch 를 사용 하여 예외 처리를 해줘야 한다. 

```java
public class Service {  
    Client client = new Client();  
  
    /**  
     * 예외를 잡아서 저리하는 코드     */    
    public void callCatch(){  
        try {  
            client.call();  
        }catch (MyCheckedException e){  
            // 예외 처리 로직  
            System.out.println("예외 처리 , message :" +e.getMessage());  
        }  
        System.out.println("정상 흐름");  
    }  
  
    /**  
     * 체크 예외를 밖으로 던지는 코드     
     * 체크 예외는 예외를 잡지 않고 밖으로 던지려면 throws 예외
     * 를 메서드에 필수로 선언해야 한다     
     * */    
    public void catchThrow() throws MyCheckedException{  
        client.call();  
    }  
}

```

만약 사용하지 않을 경우 두번째 메소드은 ``catchThrow`` 처럼 상위에 다시 넘겨서 예외 처리 하도록 해야한다.

### 잘못된 Main 클래스

`service.catchThrow();` 메소드에서  Main 클래스는 다음과 같이 throws 키워드와 함께 예외를 던지게 되며 프로그램이 강제 종료 되며  `System.out.println("정상 종료");` 부분은 실행되지 못하고 프로그램은 강제 종료 될 것이다. 

그렇기 때문에 반드시 [예외처리](%EC%98%88%EC%99%B8%EC%B2%98%EB%A6%AC.md) 를 하여 프로그램이 정상 동작 하도록 해야한다.

```java
public class CheckedThrowMain {  
    public static void main(String[] args) throws MyCheckedException {  
        Service service = new Service();  
        service.catchThrow();  
        System.out.println("정상 종료");  
    }  
}
```

## 체크 예외 장 & 단점

### 장점
1. **명시적인 예외 처리**
   - 컴파일 시점에 예외 처리 누락 방지
   - 예외 상황에 대한 명확한 문서화 효과
   - 호출자에게 예외 상황을 인지시킴

2. **안정성 확보**
   - 복구 가능한 예외에 대한 처리 보장
   - 예외 처리 체계의 일관성 유지

### 단점
1. **과도한 예외 처리 코드**
   - 불필요한 try-catch 블록 증가
   - 코드 가독성 저하
   - 개발 생산성 저하

2. **유연성 부족**
   - 상위 계층으로 예외 전파 시 모든 중간 계층에서 예외 선언 필요
   - 인터페이스 설계 시 제약 발생

