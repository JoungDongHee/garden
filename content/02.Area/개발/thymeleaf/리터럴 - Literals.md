---
date: 2025 년 03 월 18 일 14 시 03 분
tags:
  - Dev
  - Literals
  - 리터럴
  - Thymeleaf
author: Joung Dong Hee
share: true
---

# 리터럴 - Literals

Thymeleaf에서 데이터가 아닌 **고정된 값**을 그대로 출력하고자 할 경우, **리터럴 문법**을 사용하여 출력할 수 있다.

Thymeleaf에서 지원하는 리터럴의 형태는 총 4가지이며, 각각 다음과 같다.

- **문자(String)**: `'hello'`
- **숫자(Number)**: `10`
- **불린(Boolean)**: `true`, `false`
- **null 값(Null)**: `null`

---

## 문자 리터럴 (String Literal)

문자 리터럴은 작은 따옴표(`'`)로 감싸서 표시해야 한다.

```html
<span th:text="'hello'"></span>
```

하지만, **문자가 하나의 단어로 이어질 경우** 작은 따옴표를 생략할 수 있다.

```html
<span th:text="hello"></span>
```

하지만, **공백이 존재할 경우** 작은 따옴표로 감싸야 한다.

```html
<span th:text="'hello world!'"></span>
```

### 변수와 문자 리터럴의 조합

데이터와 문자를 조합하여 유동적인 문자열을 만들려면 다음과 같이 사용할 수 있다.

#### **Java 코드 (Controller)**

```java
@GetMapping("/literal")
public String literal(Model model) {
    model.addAttribute("data", "Spring!");
    return "basic/literal";
}
```

#### **Thymeleaf 코드 (View)**

```html
<li>'hello ' + ${data} = <span th:text="'hello ' + ${data}"></span></li>
```

위 코드는 `data`가 `"Spring!"`일 때 다음과 같이 출력된다.

```html
<li>'hello ' + ${data} = <span>hello Spring!</span></li>

```

---

## 숫자 리터럴 (Number Literal)

숫자 리터럴은 작은 따옴표 없이 사용할 수 있다.

```html
<span th:text="10"></span>
```


또한, 변수와 함께 숫자를 사용할 수도 있다.

#### **Java 코드 (Controller)**

```java
@GetMapping("/number")
public String number(Model model) {
    model.addAttribute("value", 5);
    return "basic/number";
}

```

#### **Thymeleaf 코드 (View)**

```html
<li>10 + ${value} = <span th:text="10 + ${value}"></span></li>
```

위 코드는 `value`가 `5`일 때 다음과 같이 출력된다.

```html
<li>10 + ${value} = <span>15</span></li>
```

---

## 리터럴 대체 (Literal Substitution)

위에서 `+` 연산자를 사용하여 데이터를 조합할 수도 있지만, **리터럴 대체 문법**을 사용하면 더욱 편리하게 표현할 수 있다.

```html
<li>'hello ' + ${data} = <span th:text="'hello ' + ${data}"></span></li>
```

위 코드를 **리터럴 대체 문법**을 이용해 다음과 같이 간결하게 작성할 수 있다.

```html
<li>리터럴 대체 |hello ${data}| = <span th:text="|hello ${data}|"></span></li>
```

위 코드는 `data`가 `"Spring!"`일 때 다음과 같이 출력된다.

```html
<li>리터럴 대체 |hello Spring!| = <span>hello Spring!</span></li>
```
### **리터럴 대체 문법의 장점**

- 문자열과 변수를 **더 간결하고 가독성 좋게 조합**할 수 있다.
- **불필요한 연산자(`+`)를 줄일 수 있어 코드가 직관적**이다.

---

## **추가로 알아두면 좋은 점**

1. **여러 개의 변수 조합도 가능**
    - `greeting="Hello"` / `name="John"` 이라면, 결과는 `"Hello John!"`이 된다.
```html
<span th:text="|${greeting} ${name}!|"></span>
```

2. **숫자 연산도 가능**`
    - `value=5` 라면 결과는 `"5 * 2 = 10"`이 된다.
```html
<span th:text="|${value} * 2 = ${value * 2}|"></span>
```

3. **Boolean 값도 사용할 수 있음**
    - `isAvailable=true`라면 결과는 `"Boolean 값: true"`가 된다.
```html
<span th:text="|Boolean 값: ${isAvailable}|"></span>
```

