---
date: 2024 년 12 월 01 일 18 시 12 분
tags:
  - Dev
  - Generic
  - Method
  - Java
author: Joung Dong Hee
share: true
---

# 제네릭 메서드 Generic Methd

[제네릭 Generic](%EC%A0%9C%EB%84%A4%EB%A6%AD%20Generic.md) 은 Method 에서도 사용이 가능하다. 

아래와 같이 클래스 자체에 타입매개변수를 전달하는 `T` 는 **제네릭 타입** 이며 이 글에서 말하는 제네릭 메서드 와는 다르다.

```java
GenericClass<T>
```


제네릭 메서드 를 정의 하기 위해서는 다음과 같이 반환 값 타입에 제네릭 을 정의해준다.

```java
public static <T> T genericMethod(T t){  
    System.out.println("generic print : "+t);  
    return t;  
}
```

또한 다음과 같이 제네릭 메서드의 인자 의 `타입 상한` 또한 지정이 가능하다. 다음과 같이 할경우 Number 타입만 사용가능하다.

```java
public static <T extends Number> T numberMethod(T t){  
    System.out.println("bound print : "+t);  
    return t;  
}
```


# 제네릭 메서드 와 제네릭 타입의 공존


만약 다음 코드와 같이 ComplexBox 에 제네릭 타입을 선언 한뒤 내부 에 제네릭 함수를 같이 선언 한다고 할 경우 이때 제네릭 메서드 안 의 타입 인자인 `T` 는 제네릭 메서드 내 에서만 적용 된다.

즉 위에서 선언한 제네릭 타입의 영향은 받지 않기 때문에 당연하게도 타입 상한 으로 설정한 `Animal` 도 따라가지 않는다. 

그렇기 때문에 제네릭 메서드의 `T` 는 `Object` 와 동일 하기 때문에 `getClass()` 나 `toString()` 과 같은 Object 의 기본 함수만 사용이 가능하다.



```java
public class ComplexBox<T extends Animal> {  
    private T animal;  
  
    public void set(T animal){  
        this.animal = animal;  
    }  
  
    public <T> T printAndReturn(T t){  
        System.out.println("ComplexBox.printAndReturn : "+animal.getClass().getName());  
        System.out.println("t.className : "+t.getClass().getName());  
        return t;  
    }  
}
```


* 추가적으로 위 와 같은 코드는 매우 부적절한 코드로 제네릭 메서드의 타입인자의 명칭은 제네릭 타입 의 타입 인자와 다르게 다음과 같이 선언 해야 다른 사람이 코드를 보고 실수 하지 않을수 있다.

```java
public class ComplexBox<T extends Animal> {  
    private T animal;  
  
    public void set(T animal){  
        this.animal = animal;  
    }  
  
    public <Z> Z printAndReturn(Z z){  
        System.out.println("ComplexBox.printAndReturn : "+animal.getClass().getName());  
        System.out.println("t.className : "+z.getClass().getName());  
        return z;  
    }  
}
```