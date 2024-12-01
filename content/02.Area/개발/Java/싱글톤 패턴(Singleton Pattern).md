---
date: 2024 년 12 월 01 일 19 시 12 분
tags:
  - Dev
  - Java
  - SingletonPattern
  - Singleton
  - Pattern
author: Joung Dong Hee
share: true
---

# 싱글톤 패턴

싱글톤 패턴(Singleton Pattern)은 **클래스의 인스턴스를 단 하나만 생성**하고, 이를 전역적으로 접근 가능하게 설계하는 패턴입니다. 이 패턴은 특정 리소스를 공유하거나 상태를 유지해야 할 때 유용합니다.


다음과 같이 `s1` 과 `s2` 는 서로 다른 객체(인스턴스) 이다.

```java
public class SingletonMain1 {  
    public static void main(String[] args) {  
        Singleton s1 = new Singleton();  
        Singleton s2 = new Singleton();  
        System.out.println(s1 == s2);  
    }  
}
```


> [!info] Result
> false


## 싱글톤 패턴이 필요한 이유

- **자원 절약**: 여러 객체를 생성하지 않고 하나의 인스턴스를 공유하여 메모리와 시스템 자원을 절약할 수 있습니다.
- **글로벌 상태 관리**: 애플리케이션 전역에서 동일한 상태나 설정을 공유할 수 있습니다.
- **객체 생성 비용 절감**: 복잡한 초기화 과정을 가진 객체를 매번 생성하는 비용을 줄일 수 있습니다.

예를 들어, 데이터베이스 연결 관리 객체는 한 번만 생성하여 여러 곳에서 공유하는 것이 효율적입니다.


# 싱글톤 구현 방법

싱글톤 패턴을 구현하는 방법은 다양하다. 


## Lazy Initialization (지연 초기화)

생성자 함수 에 `private` 키워드를 붙임으로서 외부에서 new 를 사용하여 객체를 생성하는 것을 불가능하게 하고

오로지 `getInstance()` 를 통해서만 객체를 생성 하도록 제한하였다. instance 의 인스턴가 없을 경우 인스턴스 를 새로 생성 하고 있는 경우 자기 자신을 반환 하도록 한다.


```java
public class Settings {  
    private static Settings instance;  
  
    private Settings() {  
  
    }  
  
    public static Settings getInstance() {  
        if (instance == null) {  
            instance = new Settings();  
        }  
        return instance;  
    }  
}
```


```java
public class App {  
    public static void main(String[] args) {  
        Settings settings = Settings.getInstance();  
        Settings settings1 = Settings.getInstance();  
  
        System.out.println(settings == settings1);  
    }  
}
```


> [!info] Result
> true

- **장점**: 객체가 처음 필요할 때 생성되므로 초기 메모리를 절약할 수 있습니다.
- **단점**: 멀티스레드 환경에서는 `if (instance == null)` 조건문이 여러 번 실행되어 **동시에 두 개 이상의 객체**가 생성될 가능성이 있습니다. 이를 **Thread Safe 하지 않다**고 합니다.


### 문제

^e6fadc

하지만 위 싱글톤 패턴에는 문제점이 존재한다. 대부분의 웹 Application 은 멀티쓰레드 에서 동작한다. 

하지만 위 방식은 쓰레드 세이프(Thread Safe) 하지 않다. 다음과 같이 쓰레드 A 와 B 가 Setting 인스턴스에 접근 할 경우 아직 쓰레드 A 가 인스턴스를 생성 하기도 전에 이미 쓰레드 B 또한 `if` 문 안 으로 들어왔을수도 있다.

결과론적으로 쓰레드 A 와 쓰레드 B 는 `new` 키워드를 사용하여 인스턴스를 사용할 것 이고 이 것은 서로 다른 인스턴스 를 만들게 된다.

![image.png](https://file-api.ksq9511.synology.me:5353/obsidian-image/20241201201605.png)



## **Thread safe initialization**

^c6aae9


위 에서 발생한 [문제점](%EC%8B%B1%EA%B8%80%ED%86%A4%20%ED%8C%A8%ED%84%B4(Singleton%20Pattern).md#^e6fadc) 을 해결하는 방법 은 `sychronized` 키워드를 사용하여 `동기화` 하도록 하여 한번에 하나의 쓰레드만 접근이 가능하기 때문에 하나의 인스턴스만 생성이 가능하다.

```java
public class Settings {  
    private static Settings instance;  
  
    private Settings() {  
  
    }  
  
    public static synchronized  Settings getInstance() {  
        if (instance == null) {  
            instance = new Settings();  
        }  
        return instance;  
    }  
}
```

- **장점**: `synchronized` 키워드를 사용하여 동기화하므로, 멀티스레드 환경에서도 안전합니다.
- **단점**: 동기화 비용 때문에 **성능 저하**가 발생할 수 있습니다. 이는 대부분의 호출에서 불필요한 동기화를 야기합니다.

### 문제

하지만 `sychronized` 를 사용하게 되면 이 동기화 하는 과정에 다른 쓰레드가 접근하지 못하도록 잠금(Lock) 을 걸기 때문에 이 과정에서 성능이 감소될수 있다. ^e90f50


## Eager Initialization

^05d03c

만약 객체를 생성함에 있어서 나중에 만들 필요없이 바로 만들어서 사용해도 된다고 할 경우 `이른 초기화` 방법을 사용하면 된다. 

이것은 위에서 언급한 [ Thread safe](%EC%8B%B1%EA%B8%80%ED%86%A4%20%ED%8C%A8%ED%84%B4(Singleton%20Pattern).md#^c6aae9) 한 방법으로  `static final` 을 사용하여 객체를 미리 생성하는 방법이다.


```java
public class Settings {  
    private static final Settings INSTANCE = new Settings();  
  
    private Settings() {  
  
    }  
  
    public static Settings getInstance() {  
        return INSTANCE;  
    }  
}
```


- **장점**: 애플리케이션 시작 시 객체를 미리 생성하므로, 동기화 비용 없이 멀티스레드 환경에서 안전합니다.
- **단점**: 객체 생성 비용이 높거나 사용하지 않는 경우 **불필요한 메모리 사용**이 발생할 수 있습니다.

### 문제

위 방법은 인스턴스를 미리 생성한다는 점 이 문제 이기도 하다. 인스턴스를 만드는 과정이 오래걸리고 메모리를 많이 잡아 먹는 과정이라고 할 경우 이 렇게 생성한 인스턴스를 사용하지 않을 경우 이 것은 메모리의 낭비로 이어지게 된다.


## Double-Checked Locking

[ 이른초기화](%EC%8B%B1%EA%B8%80%ED%86%A4%20%ED%8C%A8%ED%84%B4(Singleton%20Pattern).md#^05d03c) 를 사용하여 미리 만들지 말고 나중에 인스턴스를 사용할때 만들려고 한다. 하지만 이 경우 에는 위에도 언급 했듯 [성능이 감소](%EC%8B%B1%EA%B8%80%ED%86%A4%20%ED%8C%A8%ED%84%B4(Singleton%20Pattern).md#^e90f50) 될 우려가 있기 때문에 나온 방법이 있다.

`synchronized` 를 블럭 내부로 집어 넣음 으로서 인스턴스를 최초 실행 할때만 적용하고 이미 만들어진 이후에는 `synchronized` 를 실행하지 않음으로서 `synchronized` 에서 발생하는 성능 저하를 해결할수 있다.

체크를 두번 한다고 하여 `Double-Checked Locking` 이라고 한다.

```java
public class Settings {  
    private static volatile Settings instance;  
  
    private Settings() {  
  
    }  
  
    public static Settings getInstance() {  
        if (instance == null) {  
            synchronized(Settings.class) {  
                if (instance == null) {  
                    instance = new Settings();  
                }  
            }  
        }  
        return instance;  
    }  
}
```


- **장점**: 초기화 시 한 번만 동기화하므로, 동기화 비용을 최소화할 수 있습니다.
- **단점**: `volatile` 키워드의 이해와 구현이 복잡할 수 있습니다.
    - **추가 설명**: `volatile`은 CPU와 메모리 간의 동기화를 보장하여, 인스턴스가 올바르게 초기화되도록 합니다. Java 1.5 이상에서만 지원됩니다.

### 문제 

이 방법 의 경우에는 코드 를 이해함에 있어서 복잡한 `volatile` 라는 키워드의 메커니즘을 이해해야 할 필요가 있으며 또한 `volatile` 를 사용하기 위해서는 jdk1.5 부터 사용이 가능하기 때문에 자주 사용하는 방법은 아니다.


## Bill Pugh Solution (LazyHolder) - 권장

^563395

많이 권장 하는 방법 중 하나로 간결한 코드가 특징이다. 

`static` 내부 클래스를 사용하여 `SettingHolder` 클래스가 내부에서 호출하기 전까지는 인스턴스가 생성되지 않기 때문에 사용 가능한 방법이다.


```java
public class Settings {  
    private Settings() {  
  
    }  
  
    private static class SettingHolder {  
        private static final Settings INSTANCE = new Settings();  
    };  
  
    public static Settings getInstance() {  
        return SettingHolder.INSTANCE;  
    }  
}
```


### 문제

위 방법은 권장되는 방법이다. 하지만 완벽한 코드는 아니다 바로 Reflection API, 직렬화/역직렬화를 통해 클라이언트가 싱글톤 패턴을 망가트릴수 있기 때문이다.


## Enum 이용 - 권장

마지막으로 [Enum Type](Enum%20Type.md) 을 사용한 방법이다. Enum Type 의 변수는 `static final` 이며 한번만 초기화가 가능하기 때문에 `thread safe` 하다. 

또한 내부에 다양한 변수나 메서드를 선언함으로서 다양한 활용이 가능하다.  

[Bill Pugh Solution](%EC%8B%B1%EA%B8%80%ED%86%A4%20%ED%8C%A8%ED%84%B4(Singleton%20Pattern).md#^563395) 구현 방법에서 의 문제점을 해결한 방법으로 가장 권장된다고 보면 된다.

```java
public enum Settings {
    INSTANCE;

    private String someSetting;

    // 설정 값을 반환하는 메서드
    public String getSomeSetting() {
        return someSetting;
    }

    // 설정 값을 변경하는 메서드
    public void setSomeSetting(String someSetting) {
        this.someSetting = someSetting;
    }

    // 싱글톤 객체 사용 메서드
    public void doSomething() {
        System.out.println("Singleton instance is working!");
    }
}

```


---

# 참고

https://inpa.tistory.com/entry/GOF-%F0%9F%92%A0-%EC%8B%B1%EA%B8%80%ED%86%A4Singleton-%ED%8C%A8%ED%84%B4-%EA%BC%BC%EA%BC%BC%ED%95%98%EA%B2%8C-%EC%95%8C%EC%95%84%EB%B3%B4%EC%9E%90


https://www.youtube.com/watch?v=OwOEGhAo3pI&t=1s&ab_channel=%EB%B0%B1%EA%B8%B0%EC%84%A0