---
date: 2024 년 10 월 05 일 23 시 10 분
tags:
  - Dev
  - javaScript
  - Optional
share: true
---

# JavaScript 옵셔널 체이닝

자바스크립트 문법에는 옵셔널 체이닝(옵셔널 체이닝 연산자, `?.`)이라는 문법이 존재합니다. 

이 문법은 객체나 배열에서 발생할 수 있는 `null` 및 `undefined`를 안전하고 효율적으로 처리하기 위한 것으로, 특정 속성에 접근할 때 발생할 수 있는 오류를 방지할 수 있습니다.


다음과 같은 구조의 객체가 존재할 경우 

```javaScript
let item = {  
    price : 1  
};

```

## 사용 방법

`?.`를 사용하여 `item` 접근할 수 있습니다. 

`?.` 앞의 표현식이 `null` 또는 `undefined`일 경우, 뒤의 연산자인 `.price`는 실행되지 않고 `undefined`가 반환됩니다.

정상적으로 실행될 경우, 다음 속성인 `.price`에 정상적으로 접근되어 `1`이 콘솔 로그에 출력됩니다.

```js
const price = item?.price;  
console.log(price) // 1
```



옵셔널 체이닝을 사용하지 않을 경우, 다음과 같은 지저분한 코드를 사용하여 검증해야 할 것입니다:

```js
if(item.price !== null && item.price !== undefined) {  
      
}
```

### 복잡한 객체에 대한 옵셔널 체이닝

옵셔널 체이닝을 활용하여 더 복잡한 객체의 속성에 접근하는 방법도 있습니다. `?.`를 중첩 사용하여 활용할 수 있습니다.

```js
let obj = {
    name: '🚓',
    owner: {
        name: '엘리'
    }
};

function printName(obj) {
    const ownerName = obj?.owner?.name;
    console.log(ownerName);
}

printName(obj); // 엘리
```


`obj?.owner?.name`은 `obj` 에 객체가 존재하고(`true`), 그 안에 `owner` 객체가 존재하며(`true`), `owner` 객체에 `name` 속성이 있을 경우(`true`)에만 그 값을 반환합니다.

만약 중간에 `null`이나 `undefined`가 반환되면, 함수는 오류 없이 `undefined`를 출력합니다.

## 장점

옵셔널 체이닝의 장점은 다음과 같습니다:

- **안전한 접근:** 객체의 중첩된 속성에 접근할 때 발생할 수 있는 예기치 않은 오류를 방지할 수 있습니다. 예를 들어, `obj.owner.name`을 바로 접근하면 `obj`나 `owner`가 `null` 또는 `undefined`일 경우 오류가 발생하지만, 옵셔널 체이닝을 사용하면 오류 없이 `undefined`를 반환합니다.
    
- **가독성 향상:** 기존에는 `obj && obj.owner && obj.owner.name`처럼 여러 조건을 체크해야 했지만, 옵셔널 체이닝을 사용하면 간결하게 코드를 작성할 수 있습니다.
    

## 단점

옵셔널 체이닝은 몇 가지 단점 존재합니다. 

이 연산자는 `null`이나 `undefined`인 경우에만 동작하며, `false`, `0`, 빈 문자열(`""`) 같은 값은 여전히 _"존재하는"_ 값으로 간주됩니다. 따라서 중간에 `0` 같은 값이 반환될 경우, 이때는 정상적인 로직을 수행하지 않을 수 있습니다.

# 추가 정보

- **옵셔널 체이닝 과 Nullish Coalescing Operator:** 옵셔널 체이닝은 `?.`과 함께 `??`(null 병합 연산자)와 자주 사용됩니다. `??`는 왼쪽의 값이 `null` 또는 `undefined`일 경우 오른쪽 값을 반환합니다. 다음 코드는  `obj.property`가 `null` 또는 `undefined`일 경우 `'default value'`를 반환합니다.
```js
let value = obj?.property ?? 'default value';
```
    
- **ES2020에서 도입:** 옵셔널 체이닝은 ECMAScript 2020(ES11)에서 도입된 기능으로, 최신 자바스크립트 환경에서 지원됩니다. 따라서 오래된 브라우저에서는 호환성 문제가 발생할 수 있습니다.
    
- **Polyfill:** 구형 브라우저에서 이 기능을 사용하고자 할 경우 Babel과 같은 트랜스파일러를 사용하여 코드 호환성을 유지할 수 있습니다.

---
# 참고

https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Optional_chaining
