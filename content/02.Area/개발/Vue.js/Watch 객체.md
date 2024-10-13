---
date: 2024 년 09 월 28 일 22 시 09 분
tags:
  - Dev
  - Vue
  - Watch
  - FrontEnd
share: true
---

# Watch

*Watch* 는 문자 그대로 감시를 하는 역할 이다. 

특정한 데이터 를 계속 주시하고 있다가 값 이 조건을 만족하게 될 경우 로직에 맞춰 실행해주는 역할을 한다.

다음 코드 처럼 value 값 이 50을 초과 하게 될 경우 콜백 함수를 호출 하여 counter 를 0 으로 만들어 주 는 것을 의미 한다.

아래에서 코드에서 `this.counter = 0;` 가 아닌 위에서 *this 를 할당 that 에 재할당 받는 이유는 `if { }` 블록 내부에서 호출 하기 때문* 이다 .

이는 자바스크립트 의 특징중 하나로 this 가 가르키는 것은 블록 내부를 가르킨다. 그렇기 때문에 if 절 내부에서 `that.counter` 를 사용할 경우 data 프로퍼티의 객체가 아닌 전혀 엉뚱한 곳을 참조하기 때문에 재할당을 한 것 이다. 
 
```javaScript
watch: {  
	counter(value){
	const that = this;
	if(value>50){  
		setTimeout(function () {  
	      that.counter = 0;  
	    },2000)  
	  }  
	}
}
```

Watch 의 함수 명은 data 프로퍼티 와 동일 하게 작성 해야 한다. 

다음 코드와 같이 감시하고자 하는 data 프로터티의 명 과 동일하게 작성해야 한다. 
만약 data 프로퍼티 에서 name 의 값이 변경 될 때 특정한 로직을 수행하고자 한다면 

watch 객체 내부에 도 **name** 으 로 작성을 해야만 한다.

```javaScript
data() {  
  return {  
    counter: 0,  
    name : '',  
    lastName : '',  
    confirmedName : '',  
  };  
},
```


또한 watch 는 인자를 전달 받을수 있다.  총 2개의 인자를 전달 받을수 있으며 

`첫번째는 변경된 값 을 인자로 가지고 있다` , `두번째 인자는 변경되기 이전의 값을 가지고 있다.`

```javaScript
name(newVal, oldVal) {  
  if (newVal === '') {  
    this.fullName = '';  
  }else{  
    this.fullName = newVal + ' ' + this.lastName;  
  }  
},  
lastName(newVal, oldVal) {  
  if (newVal === '') {  
    this.fullName = '';  
  }else{  
    this.fullName = this.name + ' ' + newVal;  
  }  
}
```


 Watch 는 템플릿 내부에서 직접 사용지 않는다. 특정 데이터가 변경되면 실행하는 http 요청 등 을 Watch 를 사용하면 유용하다.