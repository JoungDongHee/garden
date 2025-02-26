---
date: 2025 년 02 월 20 일 15 시 02 분
tags:
  - Dev
  - RequestMapping
  - RequestParam
  - RequestBody
  - ResponseBody
author: Joung Dong Hee
share: true
---

# @RequestMapping
 
`@RequestMapping` 애노테이션은 클라이언트가 요청한 `URL`에 메서드를 매핑하기 위한 애노테이션으로, 애플리케이션의 동작과 함께 [DispatcherServlet](DispatcherServlet.md)에서 호출하기 위해 `HandlerMapping`에 등록됩니다.

## @RequestMapping 사용방법


```java
@RequestMapping(value = "/test", method = RequestMethod.GET)
public void test() {
    // 요청 처리 로직
}
```

`@RequestMapping` 애노테이션은 많이 사용하는 인자 값으로 `value`와 `method`가 존재합니다.
- **value**: 사용자의 URL을 매핑하기 위한 값으로, 사용자가 `/test`를 입력하면 해당 메서드가 실행됩니다.
    - 복수의 URL을 설정할 수도 있습니다. 예: `value = {"/test", "/test1"}`
- **method**: HTTP 메서드(GET, POST, PUT, DELETE, PATCH 등)를 정의할 수 있습니다.
    - `method`를 정의하지 않으면, 어떠한 HTTP 방식으로도 호출이 가능해집니다.

### @RequestMapping 개선


과거에는 `@RequestMapping` 애노테이션을 사용하고, 직접 `RequestMethod`를 내부 인자로 사용하여 [Http Method](Http%20Method.md)를 정의했습니다. 그러나 개발자들이 이를 일일이 선언하는 것이 번거롭다고 느끼면서, `@GetMapping`, `@PostMapping`, `@PutMapping`, `@DeleteMapping`, `@PatchMapping` 등과 같은 더 직관적인 애노테이션이 등장하게 되었습니다.

#### @GetMapping 예시

```java
@Target({ElementType.METHOD})  
@Retention(RetentionPolicy.RUNTIME)  
@Documented  
@RequestMapping(  
    method = RequestMethod.GET  
)  
public @interface GetMapping {  
}
```

위 예시에서 볼 수 있듯이, `@GetMapping`은 내부적으로 `@RequestMapping(method = RequestMethod.GET)`을 사용하는 것으로, GET 요청을 처리할 때 더 간결하고 명확한 구문을 제공합니다.


> [!tip] **TIP** 
> 애노테이션에서 `@Target({ElementType.METHOD})`는 해당 애노테이션이 적용될 수 있는 범위를 의미합니다. `METHOD`는 메서드 레벨에만 적용 가능하며, 클래스 레벨에는 적용할 수 없습니다.
> 
> 반면, `@RequestMapping`의 `@Target`은 `@Target({ElementType.TYPE, ElementType.METHOD})`로 설정되어 있어, 클래스 레벨과 메서드 레벨 모두에서 사용할 수 있습니다. 이를 통해 클래스에 대해 공통의 요청을 처리하거나, 특정 메서드에서만 요청을 처리할 수 있습니다.
---


# 추가로 알아두면 좋은 점


## 1 .**`@RequestParam` 사용**:

- URL에 포함된 파라미터를 메서드의 인자로 받을 때 `@RequestParam` 애노테이션을 사용할 수 있습니다.

```java
@RequestMapping(value = "/greet", method = RequestMethod.GET)
public String greet(@RequestParam String name) {
    return "Hello, " + name;
}
```

- `/greet?name=John`으로 요청하면 "Hello, John"이 반환됩니다.

## 2. **`@RequestBody`와 `@ResponseBody`**:

- `@RequestBody`는 요청 본문에 담긴 데이터를 객체로 변환하여 메서드 파라미터로 전달할 수 있습니다.
- `@ResponseBody`는 메서드의 반환 값을 HTTP 응답 본문으로 작성하게 해줍니다.

```java
@RequestMapping(value = "/user", method = RequestMethod.POST)
@ResponseBody
public User createUser(@RequestBody User user) {
    return user; // 클라이언트에게 JSON 형태로 반환
}
```

## 3. **클래스 레벨에서의 `@RequestMapping` 사용**

* `@RequestMapping`은 클래스 레벨에서도 사용 가능하여, 클래스 내의 모든 메서드에 대한 기본 URL을 지정할 수 있습니다.

```java
@RequestMapping("/api")
public class ApiController {
    
    @RequestMapping("/test")
    public void test() {
        // /api/test 요청 처리
    }
}
```

위와 같이 클래스 레벨에서 `@RequestMapping("/api")`를 설정하고, 각 메서드에서 세부 URL을 지정할 수 있습니다.