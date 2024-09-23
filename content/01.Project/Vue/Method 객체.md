---
create: 2024-09-19
tags:
  - Vue
  - method
  - FrontEnd
---
# Method 

Vue 에서 *Method* 객체는 이벤트(ex)클릭시 상태변환) 혹은 특정한 함수를 호출 할때 사용한다. 

다음과 같이 `outputGoal()` 함수는 자바스크립트의 내장 함수를 통해 랜덤으로 숫자를 생성한다. 
 
```javaScript
const app = Vue.createApp({  
    // data 프로퍼티 함수는 오로지 객체만을 반환한다.  
    // data 는 이미 정의된 함수로 사용자가 임의로 수정할수 없다.    
    data(){  
        return {  
            courseGoalA : 'Finish the cource and Learn Vue',  
            courseGoalB : 'Master Vue and Build Amazing Apps!',  
            vueLink : 'https://vuejs.org'  
        }  
    },  
    methods: {  
        outputGoal(){  
            const randomNumber = Math.random();  
            if(randomNumber < 0.5){  
                return this.courseGoalA  
            }else{  
                return this.courseGoalB  
            }  
        }  
    }  
});

```

그리고 이를 호출 하기 위해서  `{{outputGoal()}}` 와 같이 보간법 을 사용하여 return 받은 숫자를 그대로 화면에 표출한다.

```html
    <section id="user-goal">  
        <h2>My Course Goal</h2>  
        <p>{{outputGoal()}}</p>  
        <p>Lean more <a v-bind:href="vueLink">about Vue</a>.</p>  
    </section>
```

보간법은 오로지 vue 가 제어하는 화면에서만 적용되는 것으로  만약 vue 가 제어하지 않는 화면일 경우 보간법 문자 그대로 출력한다.

Method 는 컴포넌트의 `내부에서 호출된 모든 메서드 조건 여부 확인 없이 매번 재실행한다.`
즉 Method 에 불필요한 함수가 많이 있을 경우 이러한 모든 메서드를 재실행 하기 때문에 불필요한 리소스가 소모된다.

다음과 같이 Method 객체 내부에는 outputFullName 함수가 존재하고 html 에서는 이를 호출 하고 있다. 

단순하게 전달 받은 이름 과 `Schwarzmuller` 을 합쳐 텍스트를 반환 하는 것 이다.

하지만 Method 는 위에서 언급했듯이 화면이 랜더링 될때마다 무조건 재 실행 한다. 

만약 `addCounter(5)` 함수를 실행하여 `counter` 의 값을 변경하게 될 경우 아래의 html 화면은 랜더링 될 것이며  그 와 함께 *불 필요한 outputFullName() 함수또한 호출 하게 될것*이다.

그렇기 때문에 이러한 연산 혹은 변수 를 return 하는 함수의 경우에는 [[Computed 객체]] 객체에 정의를 해야 한다.
 
```html
    <section id="events">  
      <h2>Events in Action</h2>  
      <button @click="addCounter(5)">Add 5</button>  
      <button @click="deleteCounter(5)">Subtract 5</button>  
      <p>Result: {{ counter }}</p>  
        <input type="text" v-model="name" >  
        <input type="text" v-model="lastName" >  
        <button @click="resetInput">Reset Input</button>  
        <p>Your Name : {{ outputFullName() }}</p>  
    </section>
```


```javaScript
methods: {  
 outputFullName() {  
    if (this.name === '') {  
      return ""  
    } else {  
      return this.name + ' ' + 'Schwarzmuller'  
    }  
  },  
  resetInput() {  
    this.name = '';
  },   
    this.confirmedName = this.name;  
  },  
  addCounter(num) {  
    this.counter += num;  
  },  
  deleteCounter(num) {  
    this.counter -= num;  
  },  
}

```