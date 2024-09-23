[[Enum Type 을 사용하는 이유#^02b194|결론]]  적 으로 우리는 회원 등급을 안전하게 관리하기 위해서는 2가지 조건이 필요하다.

* 값에 대한 제한 을 둬야한다.
	* 회원 등급은 BASIC , GOLD , DIAMOND 만 입력이 되어야 하며 반드시 대문자로 관리된다.
* 타입에 대한 제한을 둬야 한다. ^b9e0cf
	* 회원 등급은 특정 Class 타입 이외에 다른 값이 입력 되지 못하도록 해야한다.

위 사안을 유의 하면서 다음 코드를 보자

# 타입 안전 열거형 패턴 구현

다음 코드는 회원 등급을 관리하는 클래스 이다. 각 회원 등급은 상수로 `static final` 을 사용하여 상수로 선언 하며 각 상수는 `서로 다른 인스턴스를 가지도록 new 를 사용하여 선언 한다.`

``` java 
public class ClassGrade {  
    public static final ClassGrade BASIC = new ClassGrade();  
    public static final ClassGrade GOLD = new ClassGrade();  
    public static final ClassGrade DIAMOND = new ClassGrade();  
  
    // private 생성자 추가  
    private ClassGrade(){  
        // 외부에서 생성은 불가능 하며 private  을 선언함으로서 내부에서만 생성 가능  
    }  
}
```

^301f95

이렇게 선언하면 그림 상 으로는 다음과 같다. 각 회원 등급은 같은 클래스 타입의 서로 다른 인스턴스를 참조하고 있는 객체가 생성된다.

![[Pasted image 20240625231700.png]]

이렇게 선언한 [[Enum Type 직접 구현#^301f95|ClassGrade]] 를 사용하여 `DiscountService` 를 구현하면 다음과 같은 코드가 된다. 

보시다 시피 기존에 String 이 들어가던 부분에 ClassGrade 만 들어 올수 있도록 선언이 되었다. 이로서 우리는 처음에 제시했던 [[Enum Type 직접 구현#^b9e0cf| 타입 제한]] 에 대해 해결할수 있게 되었다. 또한  [[Enum Type 직접 구현#^301f95|ClassGrade]] 내부에는 private 생성자를 사용하여 외부에서 classGrade 를 생성하여 사용하지 못하도록 제한을 했다.

```java
public class DiscountService {  
    public int discount(ClassGrade classGrade , int price){  
        int discountPercent = 0;  
  
        if(classGrade == ClassGrade.BASIC){  
            discountPercent = 10;  
        }else if(classGrade == ClassGrade.GOLD){  
            discountPercent = 20;  
        }else if(classGrade == ClassGrade.DIAMOND){  
            discountPercent = 30;  
        }else {  
            System.out.println("할인 X");  
        }  
  
        return price * discountPercent / 100;  
    }  
}
```


# 타입 안전 열거형 패턴 장점 

이렇게 타입 안전 열거형 패턴을 사용하면 다음과 같은 이점이 존재한다.
1. 타입안정성 향상 : 정해진 객체만 입력 이 가능하기 때문에 잘못된 값이 들어오는 것 을 사전에 방지할수 있다.
2. 데이터의 일관성 : 정해진 객체 에 정해진 값 만 사용할수 있기 때문에 일관성 이 보장된다.

