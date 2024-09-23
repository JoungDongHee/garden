# Enum Type 을 사용하는 이유 

Enum Type 을 알기 전에 다음 코드를 살펴보자 다음 코드는 각 회원의 등급에 따라 할인율을 적용하는 코드이다.  

해당 코드 는 문제가 없어보인다. 회원 등급의 문자열이 **정상적** 으로 입력이 된다면 말이다. 
하지만 다른 개발자가 실수로 회원의 등급을 소문자 로 입력을 하거나 오타가 발생할 경우 해당 할인율은 정상적으로 적용이 되지 않는다. 

다음 과 같이 문자열을 사용하여 등급 을 관리하게 될경우 다음과 같은 단점이 존재한다.

* 타임의 안전성 부족 : 사용자가 문자열을 입력하여 관리하는 경우 오타가 발생하기 쉽고 유효하지 않는 값이 입력될수 있다.
	* * 값의 제한 부족: String 으로 상태나 카테고리를 표현하면, 잘못된 문자열을 실수로 입력할 가능성이 있다. 예를들어, "Monday", "Tuesday" 등을 나타내는 데 String 을 사용한다면, 오타("Munday")나 잘못된 값 ("Funday")이 입력될 위험이 있다.
	* 컴파일 시 오류 감지 불가: 이러한 잘못된 값은 컴파일 시에는 감지되지 않고, 런타임에서만 문제가 발견되기 때문 에 디버깅이 어려워질 수 있다.
* 데이터 일관성 : GOLD , gold ,Gold 등 과 같이 다양한 문자열을 입력할수 있기 때문에 일관성이 떨어진다.

결국 *해결책은 BASIC ,  GOLD , DIAMOND 만 입력하도록 제한* 을 해야 한다. 하지만 자바의 문자열 은 이러한 것을 제한 할수 없기 때문에 String 타입을 사용해서는 문제를 해결할수 없다.

## Service Example 

``` java
public class DiscountService {  
    public int discount(String grade , int price){  
        int discountPercent = 0;  
  
        if(grade.equals("BASIC")){  
            discountPercent = 10;  
        }else if(grade.equals("GOLD")){  
            discountPercent = 20;  
        }else if(grade.equals("DIAMOND")){  
            discountPercent = 30;  
        }else {  
            System.out.println(grade + ":할인 x");  
        }  
  
        return price * discountPercent / 100;  
    }  
}
```

## Main 

``` java
public class StringGradeEx0_1 {  
    public static void main(String[] args) {  
        int price = 10000;  
  
        DiscountService discountService = new DiscountService();  
        int basic = discountService.discount("BASIC",price);  
        int gold = discountService.discount("gold",price);  
        int diamond = discountService.discount("DIAMOND",price);  
  
  
        System.out.println("BASIC 등급의 할인 금액 : "+ basic);  
        System.out.println("gold 등급의 할인 금액 : "+ gold);  
        System.out.println("diamond 등급의 할인 금액 : "+ diamond);  
    }  
}
```


# 솔루션 1 

위 문제를 해결하기 위한 방법 대안으로 **상수** 를 사용할수 있다. 상수는 미리 정의한 변수명을 사용할수 있으며 한번 선언하면 바뀔수 없기 때문에 문자열을 사용하는 것보다 안전하다.

``` java
public class StringGrade {  
    public static final String BASIC = "BASIC";  
    public static final String GOLD = "GOLD";  
    public static final String DIAMOND = "DIAMOND";  
}
```

^c19426

## Service Example

``` java
public class DiscountService {  
    public int discount(String grade , int price){  
        int discountPercent = 0;  
  
        if(grade.equals(StringGrade.BASIC)){  
            discountPercent = 10;  
        }else if(grade.equals(StringGrade.GOLD)){  
            discountPercent = 20;  
        }else if(grade.equals(StringGrade.DIAMOND)){  
            discountPercent = 30;  
        }else {  
            System.out.println(grade + ":할인 x");  
        }  
  
        return price * discountPercent / 100;  
    }  
}
```

^748ac3

## Main

``` java
public class StringGradeEx1_1 {  
    public static void main(String[] args) {  
        int price = 10000;  
        DiscountService discountService = new DiscountService();  
        int basic = discountService.discount(StringGrade.BASIC,price);  
        int gold = discountService.discount(StringGrade.GOLD,price);  
        int diamond = discountService.discount(StringGrade.DIAMOND,price);  
        System.out.println("BASIC 등급의 할인 금액 : "+ basic);  
        System.out.println("gold 등급의 할인 금액 : "+ gold);  
        System.out.println("diamond 등급의 할인 금액 : "+ diamond);  
    }  
}
```


위 [[Enum Type 을 사용하는 이유#^c19426|상수]] 를 사용한 코드는 String 을 사용하여 선언하는 것 보다 안전하다. 

상수의 이름을 잘못 입력 할경우 컴파일 에러가 발생하기 때문에 실수로 다른 값을 입력하거나 하는 실수가 줄어든다.

하지만 여전히 문제가 존재한다. 바로 [[Enum Type 을 사용하는 이유|Main]] 클래스 의 다음과 같이 **discount() 함수에 여전히 상수를 사용하지 않고 다른 문자를 넣어도 동작을 한다는 점** 이다.

``` java 
public class StringGradeEx1_2 {  
    public static void main(String[] args) {  
        int price = 10000;  
  
        DiscountService discountService = new DiscountService();  
  
        // 존재 하지 않는 등급 입력  
        int vip = discountService.discount("vip",price);  
        System.out.println("vip 등급의 할인 금액 : "+ vip);  
  
        //오타  
        int diamoondd = discountService.discount("DIAMOONDD", price);  
        System.out.println("diamond 등급의 할인 금액 : "+ diamoondd);  
  
        //소문자 입력  
        int gold = discountService.discount("gold", price);  
        System.out.println("diamond 등급의 할인 금액 : "+ gold);  
    }  
}
```

[[Enum Type 을 사용하는 이유#^748ac3|discount()]] 의 함수를 보면 String 타입 과 Int 타입을 받아서 사용할수 있다. 

이 부분 또한 자바의 문법상으로는 아무런 문제가 되지 않기 때문에 코드에 오류가 생길수 있다.

# 결론

결론적으로 우리는 등급을 관리하기 위해서는 **타입 제한 과 입력할수 있는 값에 대해 제한을 둬야 한다** 그리고 우리가 Enum Type  을 사용하는 이유이기도 하다. ^02b194