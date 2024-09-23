---
create: 2024-08-25
dg-publish: true
tags:
  - dev
  - javaScript
  - Map
  - IterableIterator
---
## Map

###  일반 출력
``` javaScript
const map = new Map([ ['key1', '🍎'], ['key2', '🍌'] ]); 
console.log(map); 
// 출력: Map(2) { 'key1' => '🍎', 'key2' => '🍌' }
```

### 크기 

``` javaScript
// Map의 크기 확인 
// Map에 저장된 키-값 쌍의 개수를 확인할 수 있습니다. 
console.log(map.size); 
// 출력: 2
```

### 값의 존재 여부 확인

``` javaScript
// 특정 키가 존재하는지 확인 
// 'has' 메서드를 사용해 특정 키가 Map에 포함되어 있는지 확인할 수 있습니다 
console.log(map.has('key1')); // 출력: true 
console.log(map.has('key4')); // 출력: false
```

### 값의 순회

map 에서의 값을 순회 할때는 기본적으로 제공하는 forEach 문을 사용할수도 있다.

``` javaScript
// 'forEach' 메서드를 사용해 Map 의 각 요소를 순회할 수 있습니다.
map.forEach((item) => { console.log(item); });
```

또한 **IterableIterator** 한 객체이므로 `for .. of` 와 같은 문법을 사용하여 접근이 가능하다. 

``` javaScript
// IterableIterator이기 때문에 'for...of' 문법을 사용할 수 있습니다. 
for (const mapElement of map) { console.log(mapElement); }
// 출력: // [ 'key1', '🍎' ] // [ 'key2', '🍌' ]
```

### Map의 모든 키, 값, 키-값 쌍에 접근하기
``` javaScript
console.log(map.keys()); 
// 출력: [Map Iterator] { 'key1', 'key2' }

console.log(map.values()); 
// 출력: [Map Iterator] { '🍎', '🍌' }

console.log(map.entries()); 
// 출력: [Map Entries] { [ 'key1', '🍎' ], [ 'key2', '🍌' ] }
```



### 특정 키에 대응하는 값 찾기
Map 에서 값을 찾을때는 특정 key 를 이용하여 찾으면 된다. 이때 없는 key 를 활용하여 찾을 경우 `undefined` 을 출력 한다.

``` javaScript
// 특정 키에 대응하는 값 찾기 
// 'get' 메서드를 사용해 특정 키에 대응하는 값을 가져올 수 있습니다.
console.log(map.get('key1')); // 출력: '🍎' 
console.log(map.get('key2')); // 출력: '🍌' 
console.log(map.get('key3')); // 출력: undefined (존재하지 않는 키)
```

### 값의 추가
Map 에서의 값의 추가는 `set` 를 사용하여 추가하는 것이 가능하다.

``` javaScript
// 새로운 키-값 쌍 추가 
// 'set' 메서드를 사용해 Map에 새로운 키-값 쌍을 추가할 수 있습니다.
map.set('key3', '🥝'); 
console.log(map); 
// 출력: Map(3) { 'key1' => '🍎', 'key2' => '🍌', 'key3' => '🥝' }
```

### 값 의 삭제

Map 안의 값을 삭제 할때는 `delete`  를 사용하여 삭제가 가능하다 이때 delete 에 들어갈 인자는 Map 의 Key  이다.

``` javaScript
// 특정 키-값 쌍 삭제 
// 'delete' 메서드를 사용해 특정 키-값 쌍을 Map에서 삭제할 수 있습니다.
map.delete('key3'); 
console.log(map); 
// 출력: Map(2) { 'key1' => '🍎', 'key2' => '🍌' }
```

만약 전부 삭제하고자 할경우 이때는 `clear` 를 사용한다.

``` javaScript
// 모든 키-값 쌍 삭제 
// 'clear' 메서드를 사용해 Map의 모든 키-값 쌍을 제거할 수 있습니다. 
map.clear(); 
console.log(map); 
// 출력: Map(0) {}
```


### 특징
set 에서의 중복은 참조하는 메모리의 객체 주소를 기준으로 한다. 예를들어 다음과 같이 `obj1` , `obj2` 와 같이 객체를 생성한뒤 `objs` 에 `set` 객체를 선언 한다.

new Set 으로 선언한 objs 는 다음과 같이 각 객체의 메모리 주소를 가질것 이다.  `new Set([x001, x002]);`
``` javaScript
// Set에 객체를 추가할 때는 객체의 메모리 참조 주소가 기준이 됩니다.
const obj1 = { name: '🍎', price: 8 };
const obj2 = { name: '🍌', price: 5 };
const objs = new Set([obj1, obj2]);
console.log(objs); 
// 출력: Set(2) { { name: '🍎', price: 8 }, { name: '🍌', price: 5 } }
```

그리고 obj1 의 price 를 수정한뒤 다시 **add** 를 할 경우 어떻게 될까? 

``` javaScript
obj1.price = 10; 
objs.add(obj1); 
console.log(objs);
```

**정답**은 **기존에 참조 하고 있던 obj1 의 값만 변경이 되고 새로운 객체는 추가 되지 않는다**

``` javaScript
Set(2) { { name: '🍎', price: 10 }, { name: '🍌', price: 5 } }
```

set 에 특성에는 중복을 허용하지 않는다 가 존재하기 때문에 `obj1.price = 10;` 최초에 작성한 가격의 변화만 존재하고 새로운 객체는 추가 되지 않는다.

이를 통해 set 에서의 중복은 참조하는 메모리를 기준으로 한다는 것을 알수 있다.

그렇다면 새로운 객체를 생성하여 추가하는 것은 어떨까? 

다음과 같이 obj3 객체를 새롭게 생성 하였다. obj3 은 객체의 이름만 다를뿐 obj2 와 값 과 구조는 동일 하다. 

``` javaScript
const obj3 = { name: '🍌', price: 5 }; 
objs.add(obj3);
console.log(objs);
```

하지만 이경우 obj3 는 obj2 와는 전혀 다른 객체 이다. 그렇기 때문에 다음 결과에서 볼수 있듯이 set 에는 새로운 obj3 이 추가 된것을 확인할수 있다.

``` javaScript
Set(3) { { name: '🍎', price: 10 }, { name: '🍌', price: 5 }, { name: '🍌', price: 5 } }
```