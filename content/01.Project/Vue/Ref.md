* vue 에서는 사용자가 입력한 값을 가져올때 다음 코드와 같이 사용자가 입력 한 값을 data 프로퍼티에 할당 하고 이를 다시 가져와 보여주거나 혹은 `v-model` 과 같은 방식 을 사용하여 사용자의 값을 입력 받아 왔다.
``` javaScript
data() {  
  return {  
    currentUserInput: '',  
    message: 'Vue is great!',  
  };  
},  
methods: {  
  saveInput(event) {  
    this.currentUserInput = event.target.value;  
  },  
  setText() {  
    this.message = this.currentUserInput;  
  },  
},
```

하지만 가끔은 사용자가 입력한 값을 전부 알 필요가 없을때도 있다. 이를 위해서 vue 에서는 필요할때만 데이터를 가져오는 DOM 요소로 부터 값을 가져오는 기능이 있다 이를 **Ref** 라고 한다.

다음 Html 코드와 같이 input 요소에 ref 속성을 추가한다. ref 속성은 html 속성이 아닌 vue 에서 인식 하는 속성으로 `ref=""` 안에는 개발하는 사용자가 인식할 식별자가 들어가게 된다.
여기서는 `userText` 가  식별자가 된다.

Ref 는 Html 에서 Dom 요소에 접근할때 쓰는 `document.getElementById(userText)` 와 동일한 역할을 한다
```Html
<section id="app">  
  <h2>How Vue Works</h2>  
  <input type="text" ref="userText">  
  <button @click="setText">Set Text</button>  
  <p>{{ message }}</p>  
</section>
```

vue 에서는 ref 를 감지하고 이를 내부에 저장한다. input 요소에 엑세스 하고자 한다는 점을 기억하고 javaScript 에서도 엑세스 할수 있도록 한다.

이후 다음과 같이 Vue 에서 지원하는 특수 프로퍼티인 **$** 기호를 사용하여 설정한다. 이는 내장 프로퍼티임을 말한다.
```javaScript
setText() {  
  //this.message = this.currentUserInput;  
  this.message = this.$refs.userText.value  
},
```

다음과 같이 콘솔에 찍어 보면 userText 의 속성 자체(`<input type="text">`)를  출력 할수 있다.

``` javaScript
console.log(this.$refs.userText);
```