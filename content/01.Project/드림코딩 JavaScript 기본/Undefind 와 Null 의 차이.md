---
create: 2024-08-18
dg-publish: true
tags:
  - dev
  - javaScript
  - Undefind
  - Null
---

JavaScript 에서는 `Undefind` 와 `Null` 두가지가 존재한다. 두개 다 값이 존재하지 않는건 동일하나 약간의 차이가 존재한다.

## Undefind

```javaScript
let variable;  
console.log(variable);
```

위 코드에서는 `Undefind` 가 출력 될 것이다. 

undefined는 변수나 객체의 프로퍼티가 선언되었지만 초기화되지 않은 경우에 기본적으로 할당되는 값을 의미합니다. 즉, 메모리 공간은 할당되었으나 값이 아직 존재하지 않는 상태를 의미합니다.

### ChatGpt 답변

undefined는 변수나 객체 프로퍼티가 선언되었지만 값이 없을 때, 자바스크립트 엔진이 자동으로 부여하는 값입니다. 메모리에 값이 할당되지 않은 상태를 의미합니다.

## Null

```javaScript
variable = null;  
console.log(variable);
```

자바스크립트에서의 `null`은 값이 없음을 명시적으로 나타내기 위해 개발자가 사용하는 값입니다. 이는 Java의 null과 다소 유사하지만, Java에서는 참조할 객체가 없음을 의미하는 반면, JavaScript에서는 특정 변수나 객체가 '값이 없음'을 의도적으로 표현하는 데 사용됩니다.

### ChatGpt 답변

 변수나 객체 프로퍼티가 명시적으로 "값이 없음"으로 초기화된 상태를 나타내며, 이는 특정 값이 없음을 의도적으로 표현하기 위해 사용됩니다. 메모리에서 값이 없는 상태로 초기화된 것과 유사합니다.
 
## Ex)

```javaScript
let activeItem; // 아직 활성화된 아이템이 있는지 없는지 모르는 상태  
activeItem = null; // 활성화된 아이템이 없는 상태  
  
console.log(typeof null);  
console.log(typeof undefined);
```