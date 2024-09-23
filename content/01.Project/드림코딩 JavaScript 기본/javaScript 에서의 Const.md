---
create: 2024-08-18
dg-publish: true
tags:
  - dev
  - javaScript
  - const
---

자바스크립트에서 에서는 변수를 선언할때 `let` 과 `const` 키워드를 사용하여 변수를 선언한다.

둘의 차이점은 변수의 재할당이 가능하냐 불가능 하냐의 차이이며 `let 은 재할당이 가능` 하지만 `const 는 재할당이 불가능` 자바에서의 상수와 비슷하다.

## let

```javaScript
let a = 1;
a = 2;
```

위 코드 상에서 보면 a 라는 변수에 1을 미리 선언해 값을 할당 하였고 그 다음 줄에서 a 라는 변수에 다시 `2 를 재할당 할수 있다`

## const

```javaScript
const text = 'hello world';
// text = 'hi'
```

위 const 로 선언한뒤 text 라는 변수에 'hello world' 를 할당 하게 될 경우 `text = 'hi'` 처럼 이미 선언된 text 에 는 재할당이 불가능 하다. 

이를 활용하여 다음과 같이 상수처럼 사용이 가능하다.

```javaScript
const MAX_FRUITS = 5;
```

### Object 에서의 const 

object 를 생성할 경우에도 마찬가지 로 const 키워드를 사용하여 선언이 가능하다.

```javaScript
const apple = {  
    name: 'apple',  
    color: 'red',  
    display : '🍎'  
};
```

위와 같이 선언할 경우 **apple = {}; 과 같이 객체 자체를 재할당하는 것은 불가능**합니다.

하지만 주의할 점이 있습니다. 객체 자체를 재할당하는 것은 불가능하지만, 객체 내부의 속성 값은 다음과 같이 재할당할 수 있습니다. ^d7fbc3

```javaScript
console.log(apple);  
apple.name = 'orange';  
apple.display = '🍏';  
console.log(apple); // { name: 'orange', color: 'red', display: '🍏' }
```

### freeze

만약 위에서 언급한 [[javaScript 에서의 Const#^d7fbc3 | 값의 재할당]] 을 막기 위해서는 `Object.freeze()` 메서드 를 사용하면 가능하다. 

#### EX)
```javaScript
const myObject = Object.freeze({  
    name: 'John',  
    age: 30  
});
```

위 코드처럼 `Object.freeze`를 사용해 객체를 생성하면, `myObject.name = 'Jane';`과 같이 속성을 재할당하려고 할 때 오류가 발생하거나 무시됩니다.

단, 이 또한 중첩된 객체의 경우, `Object.freeze`는 Object 가 존재하는 중첩된 Object가 있을 경우에는 중첩된 Object 의 속성은 여전히 변경이 가능하다.