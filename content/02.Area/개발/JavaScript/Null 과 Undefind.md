---
date: 2024 년 11 월 25 일 16 시 11 분
tags:
  - Dev
  - "Null"
  - Undefind
  - javaScript
author: Joung Dong Hee
share: true
---

# Null 과 Undefind

자바스크립트에서는 값의 `없음` 을 표현하는 방법이 2가지이다. `Null` 과 `Undefind` 이다.

이 둘은 값의 없음을 나타내지만 의미와 사용 방법에서 차이가 존재한다.

## Null

자바스크립트에서의 `null`은 값이 없음을 명시적으로 나타내기 위해 개발자가 사용하는 값이며 이는  [Java 의 Null](JAVA%20%EC%9D%98%20%EB%B3%80%EC%88%98%20%EA%B8%B0%EB%B3%B8%ED%98%95%20%EA%B3%BC%20%EC%B0%B8%EC%A1%B0%ED%98%95.md#^27f0aa)과 다소 유사하지만, Java에서는 참조할 객체가 없음을 의미하는 반면, 

JavaScript에서는 특정 변수나 객체가 '값이 없음'을 의도적으로 표현하는 데 사용됩니다.

```js
let variable = null; 
console.log(variable);
```

> [!info]+ Result
>  null


그렇기 때문의 Null 의 Type 을 확인하면 다음과 같이 `Object` 가 나오는게 확인이 된다.

```js
console.log(typeof null)
```


> [!info]+ Result
> object



## Undefind

undefined는 변수나 객체의 프로퍼티가 선언되었지만 초기화되지 않은 경우에 기본적으로 할당되는 값을 의미합니다. 즉, 메모리 공간은 할당되었으나 값이 아직 존재하지 않는 상태를 의미합니다.

```js
let variable;
console.log(variable);
```


> [!info] Result
> Undefind


`undefined` 의 타입을 확인해보면 어떠한 타입이나 메모리 등 할당된적이 없기 때문에 `undefined` 가 출력된다. 


```js
console.log(typeof undefined)
```


> [!info] Result
> undefined


# Best Practice

그렇다면 개발자가 의도적으로 값의 없음을 나타내기 위해서는 무엇이 좋은가? 그것은 바로 `null` 이다. 

다음은 `명시적으로 undefined` 을 사용하여 프로그래밍을 했을때이다. 

둘다 같은 `undefined` 출력하게 되겠지만 타 개발자가 보기에 `undefined` 를 사용 하여 할당한 이유를 모를것이다. 

이 것은 코드의 가독성 을 떨어틀이는 행위가 될 것이다.

```js
let foo;
console.log(foo);
```

```js
let bar = undefined;
console.log(bar);
```

그렇기 때문의 값의 없음을 표현하고자 한다면 `null` 을 사용하여 표현하도록 하자.


---

# 참고

https://helloworldjavascript.net/pages/160-null-undefined.html

https://chatgpt.com/share/67455ec7-079c-800c-a1b8-d7baadb4ebb9
