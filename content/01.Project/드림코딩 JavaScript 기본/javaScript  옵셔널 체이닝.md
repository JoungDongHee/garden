---
create: 2024-08-26
tags:
  - dev
  - javaScript
  - Optional chaining
---
# javaScript 옵셔널 체이닝

자바스크립트 문법에는 옵셔널 체이닝(Nullish Coalescing Operator, `?.`) 라는 문법이 존재한다. 객체나 배열 함수 등 에서 발생할수 있는 `null` 과 `undefined` 를 안전하고 효출 적으로 처리 하기 위한 문법으로 옵셔널 체이닝 을 사용하면 특정 속성에 접근할때 발생할수 있는 오류를 처리할수 있다.

## 사용 방법

다음과 같은 구조의 객체가 존재할 경우 

``` javaScript
let item = {  
    price : 1  
};

```

다음과 같이 `?.` 를 사용 하여 접근한다. `?.` 앞의 표현식이 `null` 또는 `undefined` 일 경우 뒤 연산자인 `.price` 는 실행 되지 않고 `undefined` 가 반환된다.

만약 `?.` 앞의 표현식이 정상적 으로 실행이 될경우 다음 속성인 `.price` 에 정상적으로 접근이 되어 `1`  이 콘솔로그에 나오게 된다.

``` javaScript
const price = item?.price;  
console.log(price)
```

만약 **옵셔널 체이닝** 을 사용하지 않을 경우 다음과 같은 지저 분한 코드를 사용하여 검증을 해야할 것이다. 
``` javaScript
if(item.price !== null && item.price !== undefined) {  
      
}
```

다음은 **옵셔널 체이닝** 을 활용하여 좀더 복잡한 객체의 속성에 접근하는 방법이다. `?.` 에 대해 중첩으로 사용하여 활용이 가능하다.

``` javaScript
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

`obj?.owner?.name`은 `obj` 객체가 존재하고(*true*), 그 안에 `owner` 객체가 존재하며(*true*), `owner` 객체에 `name` 속성이 있을 경우(*true*)에만 그 값을 반환합니다.

만약 중간에 `null`이나 `undefined` 를 반환한다면 함수는 오류 없이 `undefined`를 출력하게 됩니다.

## 장점

옵셔널 체이닝 의 장점은  다음과 같다.  
- **안전한 접근:** 객체의 중첩된 속성에 접근할 때 발생할 수 있는 예기치 않은 오류를 방지할 수 있다. 예를 들어, `obj.owner.name`을 바로 접근하면 `obj`나 `owner`가 `null` 또는 `undefined`일 경우 오류가 발생하지만, 옵셔널 체이닝을 사용하면 오류 없이 `undefined`를 반환합니다.
- **가독성 향상:** 기존에는 `obj && obj.owner && obj.owner.name`처럼 여러 조건을 체크해야 했지만, 옵셔널 체이닝을 사용하면 간결하게 코드를 작성할 수 있습니다.

## 한계

이렇게 좋아 보이는 옵셔널 체이닝 도 한계가 존재한다. 옵셔널 체이닝은 `null`이나 `undefined`인 경우에만 동작하며, 

`false`, `0`, 빈 문자열(`""`) 같은 값은 여전히 *"존재하는" 값*으로 간주된다. 

그렇기 때문에 중간에 `0` 과 같은 값이 반환 될경우 이때는 정상적인 로직을 수행하지 않을 것이다.

[[https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Optional_chaining | Mdn]]