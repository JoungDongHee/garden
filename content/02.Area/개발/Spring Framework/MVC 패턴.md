---
date: 2025 년 02 월 10 일 13 시 02 분
tags:
  - Dev
  - SpringFramwork
  - Web
author: Joung Dong Hee
share: true
---

# MVC (Model-View-Controller)

![image.png](https://file-api.ksq9511.synology.me:5353/obsidian-image/20250210130563.png)


MVC 패턴은 소프트웨어 설계에서 **관심사의 분리(Separation of Concerns)** 를 통해 유지보수와 확장성을 높이는 디자인 패턴입니다. MVC는 애플리케이션을 **Model(모델), View(뷰), Controller(컨트롤러)** 세 가지 역할로 분리하여 각 요소의 책임을 명확히 합니다.


## MVC 패턴의 개념

- **Model (모델)**: 애플리케이션의 **데이터 및 비즈니스 로직**을 담당합니다.
    
- **View (뷰)**: 사용자에게 데이터를 시각적으로 표현하는 역할을 합니다.
    
- **Controller (컨트롤러)**: 사용자의 요청을 받아 Model을 조작하고, View에 데이터를 전달하는 역할을 합니다.

## 2. 각 구성 요소의 역할

### **1) Model (모델)**

- 애플리케이션의 **데이터를 관리**하며, 비즈니스 로직을 포함할 수도 있습니다.
- 데이터베이스와의 상호작용을 담당할 수도 있으며, 도메인 로직을 포함할 수 있습니다.
#### **예제 (Java)**

```java
public class User {
    private String id;
    private String name;

    public User(String id, String name) {
        this.id = id;
        this.name = name;
    }

    public String getId() { return id; }
    public String getName() { return name; }
}
```

---

### **2) View (뷰)**

- 사용자 인터페이스(UI)를 담당하며, Model에서 가져온 데이터를 표시합니다.
- 비즈니스 로직을 포함하지 않으며, Controller를 통해 전달받은 데이터만 출력합니다.
#### **예제 (Thymeleaf 기반의 View)**

```html
<html>
<body>
    <h1>사용자 정보</h1>
    <p>아이디: <span th:text="${user.id}"></span></p>
    <p>이름: <span th:text="${user.name}"></span></p>
</body>
</html>
```

---

### **3) Controller (컨트롤러)**

- 사용자의 입력을 받고, Model을 조작한 후 View에 데이터를 전달합니다.
- Model과 View 사이의 다리 역할을 합니다.
#### **예제 (Spring Boot Controller)**

```java
@Controller
@RequestMapping("/user")
public class UserController {
    @GetMapping("/{id}")
    public String getUser(@PathVariable String id, Model model) {
        User user = new User(id, "홍길동"); // 실제 애플리케이션에서는 DB에서 데이터를 가져옴
        model.addAttribute("user", user);
        return "userView"; // View 이름 반환
    }
}
```

---

## 3. MVC 패턴을 사용할 때의 장점

- **관심사의 분리 (Separation of Concerns)**: 각 구성 요소가 독립적으로 동작하여 유지보수가 쉽습니다.
- **유연성 및 확장성**: View를 변경해도 Model과 Controller는 영향을 받지 않습니다.
- **코드의 재사용성 증가**: Model을 다양한 View에서 사용할 수 있습니다.

## 4. 추가로 알아두면 좋은 점

- **MVC 변형 패턴**
    - **MVVM (Model-View-ViewModel)**: ViewModel을 추가하여 UI 로직을 분리한 패턴 (Angular, Vue.js 등에서 사용)
    - **MVP (Model-View-Presenter)**: View와 Presenter가 상호작용하며 Model을 제어하는 패턴
- **MVC는 웹뿐만 아니라 데스크톱 애플리케이션, 모바일 애플리케이션에서도 활용됨**
- **Spring MVC, ASP.NET MVC, Django 등 다양한 프레임워크에서 채택**

---

## 5. 결론

MVC 패턴은 애플리케이션의 유지보수성과 확장성을 높이기 위해 널리 사용되는 디자인 패턴입니다. 각 구성 요소(Model, View, Controller)의 역할을 명확히 구분하여 **효율적인 코드 관리**가 가능하도록 설계하는 것이 중요합니다.