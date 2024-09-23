---
create: 2024-08-26
tags:
  - dev
  - javaScript
  - Optional chaining
---

# javaScript 단축 평가

자바스크립트의 논리 연산자는 **단축 평가**라는 특성을 가진다. 

이는 논리 연산을 수행할 때, 왼쪽 피연산자만으로 결과를 알 수 있으면 오른쪽 피연산자를 평가하지 않는다는 의미입니다.

## && (And 연산자)

^13b29f

다음과 같이 `obj1` 과 `obj2` 가 존재한다.  `let result = obj1 && obj2` 처럼 `&&` 를 사용하여 평가 할경우 `obj1` 이 *true* 일때만 `obj2` 가 평가 된다.

만약 `obj1`이 *false* 라면 `obj2`는 평가되지 않고 `obj1`의 값이 반환됩니다.

``` javaScript
const obj1 = {name: '🚗'};
const obj2 = {name: '🚓', owner: 'Ellie'};

let result = obj1 && obj2;
console.log(result); // obj2 객체가 출력됩니다.

```

## || (OR 연산자)

^ab7705

OR 연산자는 둘중 하나라도 *true* 경우 *true* 을 반환한다. 이를 활용하여 다음과 같이 `obj1 || obj2`에서 `obj1`이  *true*  라면 `obj2`는 평가되지 않고 `obj1`이 반환된다.

만약 `obj1`이 *false* 라면 `obj2`가 반환됩니다.

``` javaScript
let result = obj1 || obj2;
console.log(result); // obj1 객체가 출력됩니다.

```

## 활용방법

위에서 언급한 [[javaScript  단축 평가#^13b29f | AND]] 연산자 와 [[javaScript  단축 평가#^ab7705 | OR]] 연산자의 **단축 평가**를 활용한다면 다음과 같이 도 사용이 가능하다.

``` javaScript
// 활용 예  
// 조건이 truthy 일때 &&  뒤에 무언가를 해야하는 경우  
// 조건이 falshy 일때 ||  뒤에 무언가를 해야할 경우  
const obj1 = {name : '🚗'};  
const obj2 = {name : '🚓' , owner : 'Ellie'};

function changOwner(car){  
    if(!car.owner){  
        throw new Error("주입이 없음")  
    }  
    car.owner = '바뀐 주인';  
}  
  
function makeNewOwner(car){  
    if(car.owner){  
        throw new Error("주입이 있음")  
    }  
    car.owner = '새로운 주인'  
}  

// obj1.owner 는 false 이기 때문에 뒤에 있는 changOwner 는 평가 되지 못하고 obj1 을 반환한다. 
obj1.owner && changOwner(obj1);  

// obj2.owner 는 true 이기 때문에 뒤에 있는 changOwner 는 실행 되고 obj2.owner 는 changOwner 함수에 의해 값이 변경되어 
// { name: '🚓', owner: '바뀐 주인' } 을 반환한다.
obj2.owner && changOwner(obj2);  
console.log(obj1);  
console.log(obj2);  

//obj1.owner 는 값이 존재하지 않는 false 이기 때문에 다음 뒤에 있는 연산자인 makeNewOwner 이 실행되어 owner 에 새로운 주인 이라고 추가가 된다.
// { name: '🚗', owner: '새로운 주인' } 
obj1.owner || makeNewOwner(obj1);  

// obj2.owner 이미 true 이기 때문에 다음 연산자인 makeNewOwner 실행되지 않는다.
obj2.owner || makeNewOwner(obj2);  
console.log(obj1);  
console.log(obj2);  
  
// null 또는 undefined 인 경우를 확인할때  
let item = {price : 1};  
const price = item && item.price;  
console.log(price)
```

### 조건이 참 일때 

`obj1.owner && changOwner(obj1);`에서 `obj1.owner`이 존재할 경우(*true*) 그 뒤에 연산자인 `changOwner` 함수가 호출됩니다. 

### 조건이 거짓 일때

`obj1.owner || makeNewOwner(obj1);`에서 `obj1.owner`이 존재하지 않을 경우(*false*) 그 뒤의 연산자인 `makeNewOwner` 함수가 호출됩니다.
\
## 단축 평가를 활용한 기본값 설정

논리 연산자를 활용하여 변수에 기본 값을 설정할 수 있습니다. 이때 `||` 연산자를 사용하여, 값이 `false`로 간주될 경우 기본 값을 할당할 수 있습니다.

다음 코드에서 보다시피 print 메소드에 message 가 false 로 간주되는 값은 전부 'Hello World!' 를 기본값으로 하여 출력 한다.

그 외에 'Hi' 와 같이 값이 들어올 경우 받은 문자 그대로 출력한다.

``` javaScript
function print(message) {
    const text = message || 'Hello World!';
    console.log(text);
}

print(); // 'Hello World!' 출력
print(0); // 'Hello World!' 출력
print('Hi'); // 'Hi' 출력

```

### 중요한 차이점 ⭐⭐⭐⭐

- **기본 매개변수**와 **단축 연산자**를 사용할 때 차이점이 있습니다.
    - 기본 매개변수는 함수 호출 시 인자가 `undefined`일 때만 적용됩니다.
    - 반면, `||` 연산자는 인자가 `false`로 간주되는 값(예: `0`, `-0`, `null`, `undefined`, `''`)일 때 모두 기본 값을 할당합니다.

예를들어 다음과 같이 기본 매개변수를 할당하여 기본겂을 할당 할경우 이 경우 `print()` 와 같이 매개변수가 `undefined` 일때만 적용된다.

그 이외에 `print(0)` ,`print(null)` 과 같이 값으로 간주되는 값에 한해서는 그대로 호출 하여 콘솔 로그에 `null` 찍힌다. 
 
``` javaScript
function print(message = 'hi'){  
    console.log(message);  
}

```