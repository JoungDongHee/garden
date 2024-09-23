* 프로퍼티의 줄임말로 일종의 커스텀 html 속성으로 이해하면된다.
* 부모 에서 자식으로 통신할 때 프로퍼티를 사용한다.
	* 부모에서 자식 컴포넌트로 데이터를 전달할때
* **프로퍼티는 불변하다**
	* *부모 컴포넌트 에서 전달한 데이터는 자식 컴포넌트에서 변경할수 없다.* 

아래의 코드 처럼 특정 컴포넌트에 props 를 사용하여 데이터를 전달할수 있다.

```javaScript
<frien-contact name="julie jonse" phone-number="98764 1234 1543" email-address="julie@gmail.com"></frien-contact>
```

`frien-contact` 컴포넌트에 정의한 데이터를 컴포넌트 내부에서 사용하기 위 해서는 컴포넌트 내부에 props 를 설정해줘야 한다.

```javaScript
props: [  
    'name',  
    'phoneNumber',  
    'emailAddress'  
],
```

이때 `phoneNumber` 가 `phone-number` 가 아닌 이유는 props 로 정의한 데이터는 `this.` 와 같이 자바스크립트 객체로 접근이 가능하기 때문이다. `-` 표시가 들어갈경우 자바스크립트 문법상 에러가 발생하기 때문에 반드시 `CamlCase` 로 작성을 해야한다.  

다만 html 속성 으로 정의하는 `phone-number` 와 같이  `-`  를 넣어줘야 html 속성으로 이해하고 컴포넌트에서 자동으로 `CamlCase` 로 인식한다.

```javaScript
<template>  
  <li>  
    <h2>{{name}}</h2>  
    <button @click="toogleDetails">{{ detailisAreVisible ?'Hide' : 'Show' }} Details</button>  
    <ul v-if="detailisAreVisible">  
      <li><strong>Pthone : </strong>{{phoneNumber}}</li>  
      <li><strong>Email : </strong>{{emailAddress}}</li>  
    </ul>  
  </li>  
</template>
```

그 다음은 컴포넌트 내부에서 데이터를 사용할때 위 처럼 사용하면 그만 이다.


## 부모 컴포넌트 에서 전달한 데이터는 자식 컴포넌트에서 변경할수 없다

* 만약 부모 컴포넌트로 받은 `isFavorite` 이란 데이터를 내부에서 특정 시점에 맞춰 변경 할경우 이는 `단방향 데이터 전달 방식에 위배` 되기 때문에 오류를 야기한다.

```javaScript
toggleFavorite(){  
  if(this.isFavorite === '1'){  
    this.isFavorite = '0';  
  }else{  
    this.isFavorite = '1';  
  }  
},
```

![[Pasted image 20240910001812.png]]

* 자식 컴포넌트에서 부모에게 받은 프로퍼티를 변경하고자 할 경우 내부에 새로운 객체를 정의하고 사용해야 한다.
	* friendIsFavorite 와 같이 사용해야 한다.
* 아래의 코드를 실행하면 자식 컴포넌트 내부의 데이터는 변경 되지만 부모 컴포넌트(App) 의 데이터는 변경되지 않기 때문에 오류가 발생하지 않는다.

```javaScript
export default {  
  props: [  
      'name',  
      'phoneNumber',  
      'emailAddress',  
      'isFavorite'  
  ],  
  data(){  
    return {  
      detailisAreVisible:true,  
      friend: {  
          id: 'manuel',  
          name: 'Manuel Lornz',  
          phone : '0123 45678 90',  
          email: 'manuel@gmail.com',  
      },  
      friendIsFavorite: this.isFavorite  
    }  
  },  
  methods:{  
    toogleDetails(){  
      this.detailisAreVisible = !this.detailisAreVisible;  
    },  
    toggleFavorite(){  
      if(this.friendIsFavorite === '1'){  
        this.friendIsFavorite = '0';  
      }else{  
        this.friendIsFavorite = '1';  
      }  
    },  
  }  
};
```