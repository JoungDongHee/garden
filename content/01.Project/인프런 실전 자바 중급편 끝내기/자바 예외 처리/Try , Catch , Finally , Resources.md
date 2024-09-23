---
create: 2024-06-24
tags:
  - java
  - dev
  - try
  - catch
  - finally
  - resource
---

# Try , Catch , Finally 

예외처리를 하기 위한 자바의 문법에는 대표적으로 **Try , Catch , Finally** 가 있다.

`try` 에서는 프로그램상 **정상** 로직을 처리하는 곳 이다. `catch` 에서는 try 에서 발생할수 있는 **예외** 를 처리하는 곳 이다. 마지막으로 `finally` 는 예외가 발생해도 **반드시 실행** 이 되는 곳 이다.

```java
try {  
    // 정상 적인 프로세스  
    client.call();  
}catch (MyUncheckedException e){  
    // try 에서 발생한 예외를 catch 해서 처리한다  
    System.out.println("예외처리 message = "+e.getMessage());  
} finally {  
    System.out.println("정상 로직");  
}

```

* 여러개의 catch 를 사용할수도 있다.
* 여러개의 catch 가 존재할 경우 위에서 부터 아래대로 *순차*  적으로 예외 처리가 진행된다.
* catch 사용시 아래로 내려갈수록 포괄적으로 만들어야 한다. 
	*  **NullPointerException 보다 Exception 이 상위의 객체이기 때문에 Exception 이NullPointerException 보다 위에 있을 경우 NullPointerException 에 대한 에러처리를 진행하지 않는다.**

``` java
try {
    // 예외가 발생할 가능성이 있는 코드
} catch (NullPointerException e) {
    // NullPointerException 처리
} catch (ArithmeticException e) {
    // ArithmeticException 처리
} catch (Exception e) {
    // 모든 예외 처리 (위에서 처리되지 않은 다른 예외)
}
```

1. NullPointerException 이 발생했는지 제일 먼저 확인하다.
2. ArithmeticException 타입의 예외가 발생했는지 확인한다.
3. NullPointerException , ArithmeticException 에서 catch 를 못하여 예외처리를 못했을 경우 마지막에 존재하는 `Exception` 으로 모든 예외 처리를 한다.


# Try  with  resources

Application 에서 외부의 자원을 사용하는 경우 반드시 외부 자원을 반납 혹은 해제를 해야한다. 그렇기 때문에 반드시 finally 구문을 사용해야 한다.

하지만 try 에서 외부 자원을 사용하고 끝나면 finally 에서 외부 자원을 반납하는 패턴이 반복되면서  **자바 7**  부터는 *Try  with  resources* 라는 편의 기능을 제공한다.

말 그대로 try 에서 자원을 함께 사용한다는 뜻 으로 여기서 말하는 *자원* 은 *외부의 자원*을 의미한다

해당 기능을 사용하기 위해서는 **AutoCloseable** 인터페이스를 구현해야만 사용이 가능하다.

``` java
public interface AutoCloseable {
	void close() throws Exception;
}
```

AutoCloseable 을 구현 하면  *Try  with  resources* 를 사용할때 try 가 끝나는 시점에 자동으로 *close()* 가 실행이 되면서 자원을 반납할수 있게 된다.

다음과 같이 close 메소드를 Override 받아  Try 가 끝나면 반드시 실행 해야 하는 함수나 로직을 close() 에 구현을 한다.

``` java 
@Override  
public void close() {  
    System.out.println("반드시 실행해야 하는 함수");  
    disconnect();  
}
```

## Client Example

``` java
package exception.ex4;  
  
import exception.ex4.exception.ConnectExceptionV4;  
import exception.ex4.exception.SendExceptionV4;  
  
public class NetworkClientV5 implements AutoCloseable {  
    // 외부에서 접근하는 URL 주소  
    private final String address;  
    public boolean connectError;  
    public boolean sendError;  
  
    public NetworkClientV5(String address) {  
        this.address = address;  
    }  
  
    public void connect() throws ConnectExceptionV4 {  
        if(connectError){  
            throw new ConnectExceptionV4(address,address + " 서버 연결 실패");  
        }  
        //연결 성공  
        System.out.println(address+" 서버 연결 성공");  
    }  
  
    public void send(String data) throws SendExceptionV4 {  
        if(sendError){  
            throw new SendExceptionV4(data,address+" 서버 데이터 전송 실패 : "+data);  
        }  
        // 전송 성공  
        System.out.println(address+" 서버 데이터 전송: "+data);  
    }  
  
    public void disconnect(){  
        System.out.println(address + " 서버 연결 해제");  
    }  
  
    public void initError(String data){  
        if(data.contains("error1")){  
            connectError = true;  
        }  
  
        if(data.contains("error2")){  
            sendError = true;  
        }  
    }  
  
    @Override  
    public void close() {  
        System.out.println("반드시 실행해야 하는 함수");  
        disconnect();  
    }  
}
```

## Service Example

Service Class 에서는 try 에 ( ) 를 사용하여 NetworkClientV5 의 객체를 생성한다. 이후 생성된 객체에 대해서 각 메소드를 실행한뒤 try 를 빠저나오는 순간 NetworkClientV5 객체 에서 구현 해놓은 **close() 메소드가 실행이 된다**

``` java
public class NetworkServiceV5 {  
    public void sendMessage(String data){  
        String address = "http://example.com";  
  
        try (NetworkClientV5 client = new NetworkClientV5(address)) {  
            client.initError(data);  
            client.connect();  
            client.send(data);  
        } catch (Exception e){  
            System.out.println("[예외 확인] : " + e.getMessage());  
            throw e;  
        }  
    }  
}
```

만약 connect() 에서 예외가 발생할 경우 **close() 메소드가 먼저 실행되고** 그다음 catch 로 넘어가 예외 처리를 진행하게 된다.

## Try with resources 장 & 단점 

* 모든 리소스가 제대로 종료 될수 있도록 보장을 하며 finally 에서 실수로 누락 하는 문제들을 방지한다.
* 명시적으로 종료해야 하는 자원에 대한 코드를 적지 않기 때문에 코드가 간결해진다.
* NetworkClientV5 에 대한 모든것 이 try 내부에서만 한정되기 때문에 유지 보수 및 이슈 트레킹이 쉽다.
* 조금더 빠른 자원 해제가 가능하다. try -> catch -> finally 순 으로 자원을 반납했지만 위 방법을 사용할 경우 try 가 종료되면 바로 자원을 반납할수 있다.