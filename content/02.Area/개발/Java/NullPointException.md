---
date: 2024 년 11 월 24 일 23 시 11 분
tags:
  - Dev
  - NullPointException
  - Java
author: Joung Dong Hee
share: true
---

# NullPointException

Java에서 가장 흔히 볼 수 있는 에러 중 하나는 바로 `NullPointerException`입니다.

`NullPointerException`은 **"null"을 참조(`point`)하려고 시도했을 때 발생하는 예외**를 의미합니다. 즉, 참조할 객체가 없는데 해당 객체의 필드나 메서드에 접근하려고 하면 이 예외가 발생합니다.

## NullPointerException 예제

```Java
package ref;  
  
public class NullMain2 {  
    public static void main(String[] args) {  
        Data data = null;  
        data.value = 10;  
  
        System.out.println("Data = " + data.value);  
    }  
}
```

위 코드에서 Data 객체에 `null` 을 할당 하였다. 즉 메모리 에 아직 할당을 하지 않은 상태이다. 

그리고 `data.value` 필드에 10 이라는 값을 할당 하였다. 하지만 여전히 참조할수 있는 메모리 주소가 없다. 그렇기 때문에 10 이라는 변수를 할당 할 곳 이 없기 때문에 `NullPointException` 이 발생하고 프로그램은 종료된다.


> [!danger] Error
> Exception in thread "main" java.lang.NullPointerException: Cannot assign field "value" because "data" is null at ref.NullMain2.main(NullMain2.java:6)


자바에서 발생하는 `NullPointException` 을 해결하기 위해 java8 부터는  [Optional](Optional.md) 을 도입하여 이를 해결하였다