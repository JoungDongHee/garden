---
create: 2024-09-19
tags:
  - Vue
  - Computed
  - FrontEnd
---

# Computed 

*Computed* 는 [[Method 객체]] 와 다르게 오로지 연산 을 하기 위한 객체들로 이루어져 있다. 
여기서 말하는 연산 이라 함은 `1+2 = 3` 과 같이 계산을 한뒤 의 값을 return 하는 함수 를 지칭 하며 이러한 연산 함수 가 모인 객체가 *Computed* 이다.

아래의 코드와 같이 전달받은 `this.name` 과 `this.lastName` 을 합친뒤 이를 return 하는 함수이다.

```javaScript
fullName() {  
  if (this.name === '' || this.lastName === '') {  
    return ""  
  } else {  
    return this.name + ' ' + this.lastName;  
  }  
}
```

그리고 이러한 Computed 의 객체를 사용할때는 보간법( `{{ }}` ) 을 사용하여 호출 하면된다.

```html
    <section id="events">  
      <h2>Events in Action</h2>  
        <input type="text" v-model="name" >  
        <input type="text" v-model="lastName" >  
        <p>Your Name : {{ fullName }}</p>  
    </section>
```

fullName 는 함수가 아니기 때문에 `( )`  을 사용하여 호출하지 않는다. 이는 fullName 자체가 vue 내부에서 변수로 인식 하기 때문에 그렇다.

성능 면에서 값을 출력하는 대부분의 경우 [[Method 객체]] 보다는  computed 를 쓰는 것이 좋다. 

Computed 는 `다른 데이터를 기반으로 하는 데이터를 가져오기에 유용` 며 의존하는 데이터(ex `{{ fullName }} `)  의 데이터가 변경 되거나 할때 재실행 되기 때문이다.

Method 는 `페이지 에서 어떤 항목이 변경되든 값을 무조건 재 계산 하려는 경우` 에만 사용해야 한다. 

