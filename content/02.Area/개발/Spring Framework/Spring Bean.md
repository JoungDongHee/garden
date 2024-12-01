---
date: 2024 년 11 월 14 일 23 시 11 분
tags:
  - Dev
  - Bean
author: Joung Dong Hee
share: true
---

Java 에서는 인스턴스 생성을 위해 개발자가 직접 new 키워드를 사용하여 인스턴스를 생성한다.

```java
Book book = new Book()
```

하지만 Spring 에서는 컨테이너가 Bean 라는 객체를 직접 관리한다.객체의 생명주기를 컨테이너가 관리한다. 

객체의 싱글톤  , 프로토타입으로 만들것인지 와 같은 설정을 컨테이너 관리한다.

## 스프링의 핵심 기능 1

관점지향 컨테이너 
	- 빈을 생성 , 관리한다 
	- 관점 지향(AOP , Aspect-Oriented Programming)


```java
public class Book {  
    private String title;  
    private int price;  
  
    public Book(String title, int price) {  
        this.title = title;  
        this.price = price;  
    }  
  
    public String getTitle() {  
        return title;  
    }  
  
    public void setTitle(String title) {  
        this.title = title;  
    }  
  
    public int getPrice() {  
        return price;  
    }  
  
    public void setPrice(int price) {  
        this.price = price;  
    }  
}
```

 위 코드를 사용하여 아래와 같이 인스턴스를 생성하면 

```java
Book book = new Book("tets",20000);
```

1) new Book("tets",20000);
	1)  생성자가 호출 되면 Heap 메모리에 인스턴스가 생성된다.
2) book 은 1번에서 생성한 인스턴스를 참조한다.
	1) book 은 참조 변수로 레퍼런스 변수 이다.

프로그래머가 직접 생성한 객체는 Bean 이 아니므로 Spring 컨테이너에서 관리하지 않는다.

```java
public class BookExamp01 {  
    public static void main(String[] args) {  
        Book book = new Book("java",20000);  
        System.out.println(book.getTitle());  
        System.out.println(book.getPrice());  
    }  
}
```



## Bean 을 만들때 규칙

- 기본 생성자가 있어야 classLoader 를 통해서 만들수 있다.