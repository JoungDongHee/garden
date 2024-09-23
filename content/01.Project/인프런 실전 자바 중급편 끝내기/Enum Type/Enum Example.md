
## Enum Example 
```java

public enum Grade {  
    //등급별 할인율  
    BASIC(10) , GOLD(20) , DIAMOND(30), ULTRA(40);  
  
    // 할인율  
    private final int discountPercent;  
    // 가격  
    private int price;  
  
    // 생성자 호출시 할인율 적용  
    Grade(int discountPercent) {  
        this.discountPercent = discountPercent;  
    }  
  
    // 할인율 가져오기  
    private int getDiscountPercent() {  
        return discountPercent;  
    }  
  
    // 할일율 이 계산된 가격   
	public int getPrice(int price) {  
        return price * getDiscountPercent() / 100;  
    }  
}

```

위 코드는 enum 을 활용한 등급별 할인율 과 할인율이 계산된 가격의 메소드가 들어 있는 코드이다. 

enum 는 각각의 인스턴스 생성시 각 등급별 `할인율(discountPercent)`을 인자로 하여 생성을 한다.  

또한 Grade 클래스 내부에는 getPrice 를 public 으로 선언하여 할인율이 적용된 금액을 가져올수 있다.

## Main Example

``` java
public class ClassGradeEx3_4 {  
    public static void main(String[] args) {  
        int pirce = 10000;  
  
        for(Grade grade: Grade.values()){  
            printDiscount(grade,pirce);  
        }  
    }  
  
    private static void printDiscount(Grade grade , int price){  
        System.out.println(grade.name() + " 등급의 할인 금액 :"+grade.getPrice(price));  
    }  
}
```

