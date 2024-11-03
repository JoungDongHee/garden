---
date: 2024 년 11 월 03 일 20 시 11 분
tags:
  - Dev
  - try
  - catch
  - finally
  - Resource
  - Java
  - 예외처리
author: Joung Dong Hee
share: true
---

# Java의 예외 처리: Try, Catch, Finally, Resources


## Try-Catch-Finally 기본 구조

[예외처리](%EC%98%88%EC%99%B8%EC%B2%98%EB%A6%AC.md)를 위한 기본적인 구문으로 Try, Catch, Finally가 있습니다.

- `try`: 예외가 발생할 수 있는 코드 블록
- `catch`: try 블록에서 발생한 예외를 처리하는 블록
- `finally`: 예외 발생 여부와 관계없이 반드시 실행되는 블록

```java
try {
    // 정상 프로세스 실행
    client.call();
} catch (MyUncheckedException e) {
    // 예외 처리 로직
    System.out.println("예외처리 message = " + e.getMessage());
    // 예외 로깅이나 다른 처리를 추가하는 것이 좋습니다
} finally {
    // 리소스 정리 등 반드시 실행해야 하는 코드
    System.out.println("항상 실행되는 로직");
}
```

### 다중 Catch 블록 사용시 주의사항

```java
try {
    // 예외 발생 가능 코드
} catch (NullPointerException e) {
    // 구체적인 예외 처리
} catch (ArithmeticException e) {
    // 구체적인 예외 처리
} catch (Exception e) {
    // 일반적인 예외 처리
}
```

중요한 규칙들:
1. **예외 계층 구조 고려**: 더 구체적인 예외를 먼저 캐치하고, 일반적인 예외는 나중에 캐치한다.
2. **다중 예외 처리**: Java 7부터는 하나의 catch 블록에서 여러 예외를 처리할 수 있습니다:
```java
catch (NullPointerException | ArithmeticException e) {
    // 여러 예외에 대한 공통 처리
}
```

## Try-with-Resources

Java 7부터 도입된 Try-with-Resources는 자원 관리를 자동화하는 기능이다.

Application 에서 외부의 자원을 사용하는 경우 반드시 외부 자원을 반납 혹은 해제를 해야한다. 그렇기 때문에 반드시 finally 구문을 사용해야 한다.

하지만 try 에서 외부 자원을 사용하고 끝나면 finally 에서 외부 자원을 반납하는 패턴이 반복되면서  **Java 7**  부터는 *Try  with  resources* 라는 편의 기능을 제공한다.

말 그대로 try 에서 자원을 함께 사용한다는 뜻 으로 여기서 말하는 *자원* 은 *외부의 자원*을 의미한다

해당 기능을 사용하기 위해서는 **AutoCloseable** 인터페이스를 구현해야만 사용이 가능하다

### Example

- `AutoCloseable` 인터페이스를 구현한 클래스에서 사용 가능
- try 블록이 종료될 때 자동으로 `close()` 메서드 호출
- 여러 리소스를 한번에 관리 가능하다.

`NetworkClientV5` 는 `AutoCloseable` 의 구현 체로서 `close()` 메소드를 [ 오버라이딩](%EC%98%A4%EB%B2%84%EB%9D%BC%EC%9D%B4%EB%94%A9(Overriding)%20%EA%B3%BC%20%EC%98%A4%EB%B2%84%EB%A1%9C%EB%94%A9(Overloading).md#^b6c673%20) 을 하고 있다.


```java
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

`NetworkServiceV5` 에서는 Try 에서 `NetworkClientV5` 객체를 생성한뒤 각각의 메소드를 실행한다. 각 메소드를 실행한뒤 try 를 빠저나오는 순간 NetworkClientV5 객체 에서 구현 해놓은 **close() 메소드가 실행이 된다**

만약 connect() 에서 예외가 발생할 경우 **close() 메소드가 먼저 실행되고** 그다음 catch 로 넘어가 예외 처리를 진행하게 된다.

```java

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


### Try-with-Resources의 장점:

1. **자동 리소스 관리**: 명시적인 `close()` 호출 불필요
2. **안전성**: 예외 발생 시에도 리소스 해제 보장
3. **가독성**: 코드가 더 간결하고 명확해짐
4. **효율성**: 리소스 해제 타이밍 최적화

### 주의사항:
1. `close()` 메서드에서 예외가 발생할 수 있으므로, 적절한 예외 처리 필요
2. 여러 리소스를 사용할 때는 선언된 순서의 역순으로 `close()` 호출
3. 예외 발생 시 처리 순서:
   - try 블록의 코드 실행 중 예외 발생
   - 리소스의 `close()` 메서드 호출
   - catch 블록에서 예외 처리

### Example  2:
```java
public void processFile(String path) {
    try (BufferedReader reader = new BufferedReader(new FileReader(path))) {
        String line;
        while ((line = reader.readLine()) != null) {
            // 처리 로직
        }
    } catch (IOException e) {
        logger.error("파일 처리 중 오류 발생", e);
        throw new RuntimeException("파일 처리 실패", e);
    }
}
```

### 예외 처리 시 추가 고려사항:
1. **예외 전환**: 하위 계층의 예외를 상위 계층에 맞는 예외로 변환
```java
try {
    // 데이터베이스 작업
} catch (SQLException e) {
    throw new ServiceException("서비스 처리 중 오류", e);
}
```

2. **예외 로깅**: 적절한 로깅 레벨과 컨텍스트 정보 포함
```java
try {
    // 비즈니스 로직
} catch (Exception e) {
    logger.error("작업 실패: {}", operation, e);
    throw e;
}
```

3. **예외 억제**: Try-with-Resources 사용 시 발생할 수 있는 억제된 예외 처리
```java
try (Resource resource = new Resource()) {
    // 주 예외 발생
} catch (Exception e) {
    Throwable[] suppressed = e.getSuppressed();
    // 억제된 예외 처리
}
```


---

# 참고

https://www.inflearn.com/course/%EA%B9%80%EC%98%81%ED%95%9C%EC%9D%98-%EC%8B%A4%EC%A0%84-%EC%9E%90%EB%B0%94-%EC%A4%91%EA%B8%89-1
