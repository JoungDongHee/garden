---
date: 2024 년 11 월 24 일 22 시 11 분
tags:
  - Dev
author: Joung Dong Hee
share: true
---

# JAVA 의 변수 기본형 과 참조형

자바에서는 2개의 데이터 타입이 존재한다. boolean , char , int , long , float 와 같은 형태의 **기본형 데이터** 타입이며 또 다른 하나는 String , Integer , Long 와 같은 **참조형 데이터** 타입이다.

## 기본형 

^66ee1c

기본형 안 에서는 다음과 같이 구분을 할수 있다. 

1. 논리형 : boolean
2. 문자형 : char
3. 정수형 : 
	1. byte 
	2. short
	3. int
	4. long
4. 실수형: 
	1. float
	2. double

### 기본형 특징

1. 소문자로 시작
2. 기본값이 존재한다 . 예를들어 int 의 경우 변수를 선언만 하고 값을 할당하지 않을시 기본값으로 0 을 가지고 있다.
3. 변수의 선언과 함께 동시에 생성된다.
4. 기본형은 + , - 와 같은 값을 그대로 계산해 사용할수 있다.


## 참조형

^09e48c


참조형 데이터 타입은 기본형 과 다르게 **참조** 하는 메모리 의 주소를 가지고 있는 타입을 의미한다. 

예 를 들어 기본형 의 경우 변수 i 에 값 자체인 `0` 을 가지고 있는 반면   

```java
int i = 0;
```

다음과 같인 String 형태의 참조형 타입 변수를 선언할경우 `String x001 = {"학생1","학생2"}` 와 같은 형태의 구조라고 설명할수 있다. 

```java
String[] name = {"학생1","학생2"}
```


### Null 

Java 의 Null 은 참조형 변수에서 가리키는 대상이 없다면 null 이라는 값을 반환한다. 
Null 은 값이 존재하지 않는다는 뜻이다. 


```Java
package ref;  
  
public class NullMain1 {  
    public static void main(String[] args) {  
        Data data = null;  
        System.out.println("1. Data = "+data);  
  
        data = new Data();  
        System.out.println("2. Data = "+data);  
    }  
}
```

위 와 같이 Data 라는 객체에 null 을 대입 할경우 아직 참조하는 메모리가 존재하지 않기 때문에 첫번째 의 `Print` 또한 null 을 반환 할 것이다.


하지만 두번째의 `new Data()` 를 선언하는 순간 이 것 은 메모리 변수에 새롭게 할당하는 것이 됨으로 두번째의 `Print` 에서는 참조하는 메모리 값을 반환 할 것이다.

```Java
1. Data = null
2. Data = ref.Data@1e67b872
```


### 참조형 의 대입

참조형에 대입을 할경우 조심해야한다. 

```Java
package class1;  
  
public class test {  
    public static void main(String[] args) {  
        String[] name = {"학생1","학생2"};  
  
        String[] name1 = name;  
  
        System.out.println(name);  
        System.out.println(name1);  
    }  
}
```


위 와 같이 String 배열에 name 을 선언하고 name 이라는 변수에는 `{학생1,학생2}` 와 같이 값이 있다고 가정 한다.

이후 다시 String 배열에 name1 이라는 변수를 선언하고 해당 변수에 위에서 선언한 name 을 대입 한다고 가정한다.

이 경우 name 의 참조 값이 name1 에도 그대로 대입이 되기 때문에 서로 같은 값을 바라보게 된다.

```Java
[Ljava.lang.String;@2ef9b8bc
[Ljava.lang.String;@2ef9b8bc
```

그렇기 때문에 만약 `name1` 의 값을 임의로 바꿀경우 같은 참조값을 사용하고 있는`name` 의 값도 변경이 된다.

```Java
package class1;  
  
public class test {  
    public static void main(String[] args) {  
        String[] name = {"학생1","학생2"};  
  
        String[] name1 = name;  
  
        System.out.println(name[1]);  
        System.out.println(name1[1]);  
  
        System.out.println("----------------------");  
  
        name1[1] = "학생3";  
  
        System.out.println(name[1]);  
        System.out.println(name1[1]);  
    }  
}
```

```Java
학생2
학생2
----------------------
학생3
학생3
```

위 처럼 `name1[1]` 의 배열을 변경했지만 기존의 `name[1]` 의 배열도 같이 변경된 것을 확인할수 있 


### 참조형 특징


1. 대문자로 시작한다 . String , Integer , Long
2. 메모리의 주소 값을 참조한다.
3. 참조형 변수는 기본값이 Null 이 다.
	1. 가르키는 주소가 없는 것 을 의미 하며 자바에서는 흔히 발생하는 [NullPointException](NullPointException.md) 이 발생하는 주 원인이다. 


--- 

# 참고


https://www.inflearn.com/course/%EA%B9%80%EC%98%81%ED%95%9C%EC%9D%98-%EC%8B%A4%EC%A0%84-%EC%9E%90%EB%B0%94-%EA%B8%B0%EB%B3%B8%ED%8E%B8/dashboard
