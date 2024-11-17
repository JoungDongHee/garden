---
date: 2024 년 11 월 15 일 00 시 11 분
tags:
  - Dev
  - SpringFramwork
  - Ioc
  - ioC
  - DI
author: Joung Dong Hee
share: true
---

# IoC(Inversion of Control)

직역하면 "제어 의 역전" 이라는 의미 이다. 프로그래밍 관점으로 얘기한다면 **메소드,객체 생성 , 생명 주기 등 을 사용하는 개발자가 정하는 것 이 아니라 외부에서 결정되는 것을 말한다.**

자바에서는 다음과 같이 `new` 키워드를 사용하여 인스턴스를 생성 및 사용을 한다.  

```java
Book book = new Book()
```

그리고 이렇게 생성된 객체를 사용하고자 할때 다음과 같이 `Book 클래스` 에 있는 메소드를 직접 호출하고 사용한다.

```java
public static void main(String[] args) {  
	Book book = new Book("java",20000);  
	System.out.println(book.getTitle());  
	System.out.println(book.getPrice());  
}  
```

하지만 [Spring Framework](Spring%20Framework.md) 에서는 객체의 생성 및 관리하는 `IoC 컨테이너` 가 존재하며 이 컨테이너 내부에서 관리하게 된다.

## IoC 컨테이너 의 역할

IoC 의 컨테이너는 객체의 생명주기 , 설정 관리 , Bean 의 범위 설정 등을 담당하며 이렇게 `IoC 컨테이너` 에서 관리 하는 객체를 [Spring Bean](Spring%20Bean.md) 라고 한다.  

Spring 에서는 다양하게 `IoC 컨테이너` 에 `Bean` 을 등록할수 있으며 `@Component` , `@Autowired` 등과 같은 Anotaion 을 사용하여 Bean 객체로 등록이 가능하다.

```java
@Component
public class BookService {
    private final BookRepository bookRepository;

    // Constructor-based DI
    @Autowired
    public BookService(BookRepository bookRepository) {
        this.bookRepository = bookRepository;
    }

    public void getBookInfo() {
        Book book = bookRepository.findBook();
        System.out.println(book.getTitle());
        System.out.println(book.getPrice());
    }
}

```