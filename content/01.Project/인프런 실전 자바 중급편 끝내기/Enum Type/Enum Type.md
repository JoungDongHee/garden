자바 에서는 [[Enum Type 직접 구현#^301f95|타입 안전 열거형 패턴]] 을 사용자가 직접 구현하지 않아도 되도록 프로그래밍 에서 Enum Type 를 지원한다.

```java
public enum Grade {  
    BASIC , GOLD , DIAMOND;  
}
```

class 가 들어가는 자리에 **enum** 이라고만 작성이 되면 다음과 같이 사용자가 구현할 필요가 없어진다.

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

타입안전 열겨형 패턴과 마찬가지로 **외부에서 생성이 불가능하며 각 각 의 상수는 서로 다른 인스턴스를 가지게 된다.**

만약 회원 등급이 추가가 된다면 우리는 enum 에 다음과 같이 새로운 등급을 추가하면 될뿐이다.

```java
public enum Grade {  
    BASIC , GOLD , DIAMOND , PLATINUM;  
}
```

참고로 Enum 을 사용할 때에는 다음 코드에서 처럼 `import static enumeration.ex3.Grade.*`  와 같이 선언하면 좀더 가독성이 좋게 코드를 만들수 있다. 

``` java
import static enumeration.ex3.Grade.*;  
  
public class ClassGradeEx3_1 {  
    public static void main(String[] args) {  
        int pirce = 10000;  
  
        DiscountService discountService = new DiscountService();  
        int basic = discountService.discount(Grade.BASIC, pirce);  
        int gold = discountService.discount(GOLD, pirce);  
        int diamond = discountService.discount(DIAMOND, pirce);  
  
        System.out.println("BASIC 등급의 할인 금액 : "+ basic);  
        System.out.println("gold 등급의 할인 금액 : "+ gold);  
        System.out.println("diamond 등급의 할인 금액 : "+ diamond);  
    }  
}
```