---
date: 2024 년 11 월 03 일 21 시 11 분
tags:
  - Dev
  - UnCheckedExcption
  - 예외처리
  - 언체크예외
author: Joung Dong Hee
share: true
finish: true
---
# 언체크 예외(UnChecked Excption)

## UnChecked Excption

[체크 예외 (Checked Exception)](%EC%B2%B4%ED%81%AC%20%EC%98%88%EC%99%B8%20(Checked%20Exception).md) 와 정 반대의 개념으로 개발자가 명시적으로 [예외처리](%EC%98%88%EC%99%B8%EC%B2%98%EB%A6%AC.md) 를 하지 않아도 된다.

## UnChecked Exception 구현

### 사용자 정의 UnChecked Exception

다음 과 같이 `RuntimeException` 을 상속받아 `MyUncheckedException` 클래스를 생성하였다

```java
/**  
 * RuntimeException 을 상속 받은 예외ㅏ는 언체크드 예외가 된다 */
 public class MyUncheckedException extends RuntimeException {  
    public MyUncheckedException(String message) {  
        super(message);  
    }  
}
```

### Client Example

`Client` 클래스 에서는 call 를 사용하여 의도적으로 `MyUncheckedException` 를 발생시킨다. 이미 call 에서 부터 체크 예외 하고는 다른점이 존재한다. 

바로 **throws** 키워드 가 빠젔다는 것 이다. 언체크드 예외는 개발자가 명시를 안해줘도 컴파일 에서 오류 가 발생할 일이 없다. 

그리고 throws 가 생략 되면 알아서 해당 클래스 의 바깥으로 던지게 된다.

```java
public class Client {  
    public void call(){  
        throw new MyUncheckedException("ex");  
    }  
}
```


## Service Example 

`Service` 클래스 에서는 동일하게 `client.call()` 함수를 실행한다.  `CallCatch` 메소드 에서 처럼 **try , catch** 문법을 사용 하여 예외를 처리 할수도 있다.

하지만 `CallThrow()` 처럼 예외 처리를 생략할수도 있다.


```java
/**  
 * Unchecked 예외는 * 예외를 잡거나 던지지 않아도 된다 * 예외ㅏ를 잡지 않으면 자동으로 밖으로 던진다 */
public class Service {  
    Client client = new Client();  
    /**  
     * 필요한 경우 예외를 잡아서 처리할 수 있다     */    
    public void CallCatch(){  
       try {  
           client.call();  
       }catch (MyUncheckedException e){  
           System.out.println("예외처리 message = "+e.getMessage());  
       }  
        System.out.println("정상 로직");  
    }  
    /**  
     * 예외를 잡지 않아도 된다. 자연스럽게 상위로 넘어간다     
     * 체크 예외와 다르게   throws 예외 선언을 하지 않아도 된다.     
     * */    
    public void CallThrow(){  
        client.call();  
    }  
}
```


## 언체크 예외 장 & 단점

### 장점

1. **간결한 코드 작성**
    - 예외 처리가 필요한 경우에만 예외 처리 코드를 작성할 수 있음
    - `throws` 선언이 필요 없어 코드가 간결해짐
    - 불필요한 `try-catch` 블록 감소로 가독성 향상
2. **유연한 예외 전파**
    - 호출자에게 강제적인 예외 처리를 요구하지 않아, 필요한 경우에만 처리 가능
    - 설계 시 인터페이스나 API에 대한 제약이 적어, 유연한 코드 작성이 가능함

### 단점

1. **예외 처리 누락 가능성**
    - 예외 처리를 강제하지 않기 때문에 중요한 예외 처리를 누락할 위험이 있음
    - `NullPointerException` 등 런타임 오류 발생 시 원인을 찾기 어려울 수 있음
2. **안정성 저하**
    - 런타임에 예상치 못한 예외 발생 가능성이 높아져 시스템 안정성에 영향을 미침
    - 애플리케이션의 예외 처리 일관성이 떨어질 수 있음