---
date: 2024 년 02 월 05 일 23 시 24 분
tags:
  - Dev
  - Java
  - DownCasting
  - UpCasting
  - 다형성
share: true
---

자바에서 객체지향 프로그래밍을 이야기할 때 빠질 수 없는 개념 중 하나가 바로 **다형성**입니다. 
다형성은 *"다양한 형태"* 를 의미하며, 여러 타입의 객체를 같은 타입으로 다룰 수 있는 능력을 말합니다. 

예를 들어, `Parent poly = new Parent();`와 같이 부모 타입의 객체뿐만 아니라, `Parent poly = new Child();`처럼 자식 클래스의 객체도 부모 타입으로 취급할 수 있습니다. 

이를 통해 객체를 유연하게 다룰 수 있게 해줍니다.

# 다형적 참조

다음은 다형적 참조를 위한 예제 코드입니다.

## Parent class 

```java
public class Parent {  
    public void parentMethod() {  
        System.out.println("Parent.parentMethod");  
    }  
}
```

## Child class
```java
public class Child extends Parent {  
    public void childMethod() {  
        System.out.println("Child.childMethod");  
    }  
}
```


위 코드에서 `Parent`라는 부모 클래스와 `Child`라는 자식 클래스가 있습니다. `Child` 클래스는 `Parent` 클래스를 상속받았으므로, 부모 클래스의 메서드인 `parentMethod`를 사용할 수 있습니다.

## Main class # 1

```java
public static void main(String[] args) {  
    // 부모 변수가 부모 인스턴스를 참조  
    System.out.println("Parent -> Parent ");  
    Parent parent = new Parent();  
    parent.parentMethod();  
}

```

**부모 객체는 부모 인스턴스를 참조**할 수 있습니다.

**Main class # 1**에서는 `Parent` 클래스의 인스턴스를 `parent` 변수가 참조하고, `parentMethod`를 호출하여 부모 클래스의 메서드를 실행합니다.

## Main class # 2

```java
public static void main(String[] args) {  
    // 자식 변수가 자식 인스턴스를 참조  
    System.out.println("Child -> Child");  
    Child child = new Child();  
    child.parentMethod();  // 부모 클래스의 메서드 호출  
    child.childMethod();   // 자식 클래스의 메서드 호출
}

```

마찬가지로, **Main class # 2** 에서 자식 클래스도 자신의 인스턴스를 참조하여 실행할 수 있으며, 부모 클래스를 상속받았기 때문에 부모 객체에 있는 `parentMethod`도 실행할 수 있습니다.




# 다형적 참조의 메모리 구조

![다형적 참조 1.png](https://file-api.ksq9511.synology.me:5353/obsidian-image/20241005223047.png)



다형적 참조를 메모리 구조로 나타내면, `new Child()`를 사용하여 자식 인스턴스를 생성할 때 메모리에는 `Child`와 `Parent` 클래스의 인스턴스가 생성됩니다. 생성된 참조값은 `Parent poly` 변수에 저장되며, `poly.parentMethod()`를 호출하면 참조된 객체에서 부모 클래스의 메서드를 실행합니다.


# 다형적 참조의 한계

부모 클래스가 자식 클래스의 인스턴스를 참조하는 것은 가능하지만, 그 반대는 불가능합니다. 

즉, **부모 클래스의 인스턴스를 자식 클래스의 변수에 담을 수 없습니다**. 또한, 다형적 참조로 자식 인스턴스를 부모 타입으로 참조했을 경우, 자식 클래스에서 추가된 메서드는 호출할 수 없습니다.

```java
public static void main(String[] args) {  
    Parent poly = new Child();  
    System.out.println("Child -> Parent");  
    // 자식 클래스의 메서드는 호출할 수 없다  
    poly.childMethod();  // 컴파일 오류 발생
}

```

위 코드에서 `poly.childMethod()`를 호출하면 컴파일 오류가 발생합니다. `poly`는 `Parent` 타입이므로 부모 클래스에 정의된 메서드만 호출할 수 있습니다. 

`Parent` 클래스에는 `childMethod()`가 정의되어 있지 않기 때문에 호출할 수 없습니다.

## 다운캐스팅 (Downcasting)

자식 클래스의 메서드를 호출하려면 **다운캐스팅**이 필요합니다. 

다운캐스팅은 부모 타입의 참조를 다시 자식 타입으로 변환하는 것을 의미합니다. 다음과 같이 캐스팅을 통해 자식 클래스의 메서드를 호출할 수 있습니다.

```java
public static void main(String[] args) {  
    Parent poly = new Child();  
    ((Child) poly).childMethod();// 자식 클래스의 메서드를 호출하기 위한 다운캐스팅
}

```

이 코드에서는 `poly` 변수를 자식 클래스 타입으로 캐스팅하여 `childMethod()`를 호출합니다. 하지만 **다운캐스팅은 주의해서 사용해야 하며**, 참조하는 객체가 실제로 자식 클래스의 인스턴스가 아닐 경우 `ClassCastException`이 발생할 수 있습니다.