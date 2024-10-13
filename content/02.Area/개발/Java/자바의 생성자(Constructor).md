---
date: 2024 년 10 월 06 일 17 시 10 분
tags:
  - Dev
  - Java
  - Constructor
  - 생성자
share: true
---

# 생성자

**자바의 생성자(Constructors)** 는 객체가 생성될 때 호출되어 초기화를 수행하는 특수한 메서드입니다. 생성자는 클래스의 인스턴스를 생성할 때 특정 값을 초기화하거나, 필요한 설정을 하도록 돕습니다.


## 생성자의 규칙

1. 생성자의 이름은 클래스 이름과 같아야 하며, 첫 글자는 대문자여야 합니다.
    - 예: `MemberConstruct` 클래스의 생성자는 `MemberConstruct`여야 합니다.
2. 생성자는 반환 타입이 없습니다.
3. 생성자 외의 다른 부분은 일반 메서드처럼 작성할 수 있습니다.

## 생성자의 장점

1. **코드 간결성**: 생성자를 사용하면 코드가 간결해지고, 중복된 초기화 코드를 줄일 수 있습니다.
2. **일관성 유지**: 생성자를 통해 객체 생성 시 필요한 값을 반드시 설정하도록 강제할 수 있어, 객체의 일관성과 무결성을 유지할 수 있습니다.
3. **필수 요소**: 생성자는 클래스에서 필수 요소입니다. 개발자가 생성자를 정의하지 않으면, 자바 컴파일러가 자동으로 기본 생성자를 제공합니다.


## 생성자의 사용 이유

객체를 생성할 때 동시에 필요한 초기화 작업을 수행하려면 **생성자(Constructor)** 를 사용하는 것이 좋습니다. 생성자는 객체를 생성하는 시점에 값을 할당하거나 초기화 작업을 수행할 수 있어, 객체의 일관성을 유지하는 데 유용합니다.

예를 들어, `MemberInit` 객체를 생성하면서 이름, 나이, 성적을 설정하는 경우를 생각해보겠습니다.

```java
public class MemberInit {
    String name;
    int age;
    int grade;

    // 생성자를 사용하여 객체 생성 시 바로 값을 초기화
    public MemberInit(String name, int age, int grade) {
        this.name = name;
        this.age = age;
        this.grade = grade;
    }
}

```


다음은 생성자를 사용하여 객체를 생성하고 초기화하는 예시입니다.

```java
public class MethodInitMain3 {
    public static void main(String[] args) {
        // 생성자를 통해 객체 생성과 동시에 초기화
        MemberInit member1 = new MemberInit("user1", 15, 90);
        MemberInit member2 = new MemberInit("user2", 16, 80);

        MemberInit[] members = {member1, member2};

        for (MemberInit s : members) {
            System.out.println("이름: " + s.name + " 나이: " + s.age + " 성적: " + s.grade);
        }
    }
}
```

>[!result] 
>이름: user1 나이: 15 성적: 90  
>이름: user2 나이: 16 성적: 80


위 예시에서 생성자를 사용하여 각 객체가 생성되는 시점에 값을 초기화할 수 있었습니다.

