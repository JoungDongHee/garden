---
date: 2024 년 11 월 11 일 22 시 11 분
tags:
  - Dev
  - OCP
  - OOP
  - 객체지향
  - 다형성
author: Joung Dong Hee
share: true
---

# OCP 원칙

OCP (Open/Closed Principle) 원칙은 
[객체지향 프로그래밍 (Object-Oriented Programming, OOP)](%EA%B0%9D%EC%B2%B4%EC%A7%80%ED%96%A5%20%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D%20(Object-Oriented%20Programming,%20OOP).md)  프로그래밍의 5대 원칙 중 하나로, "확장에는 열려 있고, 수정에는 닫혀 있어야 한다"라는 철학을 담고 있습니다. 즉, 기존 코드를 수정하지 않고 새로운 기능을 추가할 수 있는 구조를 목표로 합니다.

* **Open** for extension : 새로운 기능을 추가하거나 변경 사항이 생겼을때 기존 코드는 확장할수 있어야 한다.
* **Close** for modification : 기존 코드는 이미 검증된 상태이므로, 추가 요구사항이 발생해도 기존 코드는 수정되지 않아야 합니다. 이를 통해 **코드의 안정성과 예측 가능성** 을 높일 수 있습니다.


## Example Code

다음과 같이 Car 라는 interface 클래스를 만들었다. 

```java
public interface Car {  
    void startEngine();  
    void offEngine();  
    void pressAccelerator();  
}
```


인터페이스를 통해 각 자동차 모델이 같은 메서드 시그니처를 가지도록 강제하여, 자동차 모델을 교체하거나 확장해도 클라이언트 코드에 영향을 최소화합니다.

### K3Car class

`K3Car`와 `Model3Car` 클래스는 `Car` 인터페이스를 구현하여 각 모델의 동작을 정의합니다. 이처럼 구체적인 구현은 인터페이스와 분리되어, 클라이언트 코드(Driver) 가 각 자동차 구현의 내부 작동 방식을 알 필요가 없습니다.

```java
public class K3Car implements Car {  
    @Override  
    public void startEngine() {  
        System.out.println("K3Car.startEngine");  
    }  
  
    @Override  
    public void offEngine() {  
        System.out.println("K3Car.offEngine");  
    }  
  
    @Override  
    public void pressAccelerator() {  
        System.out.println("K3Car.pressAccelerator");  
    }  
}
```

### Model3Car class

```java
public class Model3Car implements Car {  
    @Override  
    public void startEngine() {  
        System.out.println("Model3Car.startEngine");  
    }  
  
    @Override  
    public void offEngine() {  
        System.out.println("Model3Car.offEngine");  
    }  
  
    @Override  
    public void pressAccelerator() {  
        System.out.println("Model3Car.pressAccelerator");  
    }  
}
```



### Driver

`Driver` 클래스는 `Car` 타입의 객체만을 참조하여 작동합니다. 즉, 자동차 모델에 구애받지 않으며, 이를 통해 새로운 자동차 모델이 추가될 때 `Driver` 클래스를 수정할 필요가 없습니다.

```java
public class Driver {  
    private Car car;  
  
    public void setCar(Car car){  
        System.out.println("자동차를 설정합니다."+car);  
        this.car = car;  
    }  
  
    public void driver(){  
        System.out.println("자동차를 운전합니다.");  
        car.startEngine();  
        car.pressAccelerator();  
        car.offEngine();  
    }  
}
```


### Main 

Main 클래스에서는 위에 구현한 클래스들을 활용하여 실행한다.

```java
public class CarMain {  
  
    public static void main(String[] args) {  
        Driver driver = new Driver();  
  
        K3Car k3Car = new K3Car();  
        driver.setCar(k3Car);  
        driver.driver();  
  
        // 차량 모델 변경  
        Model3Car model3Car = new Model3Car();  
        driver.setCar(model3Car);  
        driver.driver();  
    }  
}
```



### 새로운 Class 추가 

새로운 자동차 모델이 추가될 경우에도 `Car` 인터페이스를 구현하고 `CarMain` 클래스에서 객체를 전달하기만 하면 됩니다. 

이렇게 하면 기존의 `Driver` 클래스는 변경하지 않고도 새로운 모델을 확장할 수 있습니다.

```java
public class Y3Model3Car implements Car {  
    @Override  
    public void startEngine() {  
        System.out.println("Y3Model3Car.startEngine");  
    }  
  
    @Override  
    public void offEngine() {  
        System.out.println("Y3Model3Car.offEngine");  
    }  
  
    @Override  
    public void pressAccelerator() {  
        System.out.println("Y3Model3Car.pressAccelerator");  
    }  
}
```


```java
public class CarMain {  
  
    public static void main(String[] args) {  
        Driver driver = new Driver();  
  
        K3Car k3Car = new K3Car();  
        driver.setCar(k3Car);  
        driver.driver();  
  
        // 차량 모델 변경  
        Model3Car model3Car = new Model3Car();  
        driver.setCar(model3Car);  
        driver.driver();  
  
        Y3Model3Car y3Model3Car = new Y3Model3Car();  
        driver.setCar(y3Model3Car);  
        driver.driver();  
    }  
}
```


## **Open** for extension

위 코드처럼 Car 인터페이스를 활용하면 새로운 차량을 자유롭게 추가가 가능하다. 또한 `Car` 인터페이스를 사용하는 클라이언트 코드 `Driver` 또한 `Car` 의 객체만 필요하기 때문에 새롭게 추가된 차량 또한 자유롭게 호출이 가능하다. 

이것이 **확장에 열려있다** 라는 의미이다.


## **Close** for modification

새로운 자동차를 추가할때 영향을 받는 클라이언트 코드는 바로 Car 기능을 사용하는 Driver 클래스 인다. 여기서 **Close** for modification 는 바로 Driver 클래스를 의미한다.

대신 CarMain 클래스 에서는 새로운 차를 생성하고 전달하는 코드는 추가되기 때문에  이러한 부분은 OCP 를 지켜도 변경이 필요하다.


---

# 참고


https://www.inflearn.com/course/%EA%B9%80%EC%98%81%ED%95%9C%EC%9D%98-%EC%8B%A4%EC%A0%84-%EC%9E%90%EB%B0%94-%EA%B8%B0%EB%B3%B8%ED%8E%B8/dashboard


https://inpa.tistory.com/entry/OOP-%F0%9F%92%A0-%EC%95%84%EC%A3%BC-%EC%89%BD%EA%B2%8C-%EC%9D%B4%ED%95%B4%ED%95%98%EB%8A%94-OCP-%EA%B0%9C%EB%B0%A9-%ED%8F%90%EC%87%84-%EC%9B%90%EC%B9%99