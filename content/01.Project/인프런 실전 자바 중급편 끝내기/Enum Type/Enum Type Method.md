Enum 타입 을 선언하게 될경우  java.lang.Enum 클래스 를 자동으로 상속 받기 때문에 해당 클래스에서 제공하는 기능들을 사용할수 있다.


```java
public class EnumeMethodMain {  
    public static void main(String[] args) {  
        // 모든 Enum 반환  
        Grade[] values = Grade.values();  
        System.out.println("values = "+ Arrays.toString(values));  
  
        // Enum 의 이름 과 순서 반환  
        for (Grade value : values){  
            System.out.println("name = "+value.name()+", ordinal = "+value.ordinal());  
        }  
  
        // String -> ENUM 반환  
        String input = "GOLD";  
        Grade gold =Grade.valueOf(input);  
        System.out.println("gold = "+gold);  
    }  
}
```

> [!note|result]
> values = BASIC, GOLD, DIAMOND
> name=BASIC, ordinal=0
> name=GOLD, ordinal=1
> name=DIAMOND, ordinal=2
> gold = GOLD


# 주요 메서드 

* values(): 모든 ENUM 상수를 포함하는 `배열을 반환`한다.
* valueOf(String name): 주어진 이름과 일치하는 `ENUM 상수를 반환`한다.
* name(): ENUM 상수의 이름을 `문자열로 반환`한다.
* ordinal(): ENUM 상수의 `선언 순서(0부터 시작)를 반환`한다.
* toString(): ENUM 상수의 이름을 문자열로 반환한다. name() 메서드와 유사하지만, toString() 은 직접 오버라이드 할 수 있다.