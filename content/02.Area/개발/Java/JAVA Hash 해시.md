---
date: 2024 년 12 월 29 일 22 시 12 분
tags:
  - Dev
  - Hash
  - equals
  - hashCode
author: Joung Dong Hee
share: true
---

# JAVA Hash

자바의 핵심 기술 중 하나인 **Hash**는 데이터를 효율적으로 관리하고 검색하는 데 중요한 역할을 합니다. Hash는 [Collection Framework](Collection%20Framework.md)에서도 다양하게 활용됩니다.특히 **HashMap**, **HashSet**, **Hashtable**과 같은 자료구조에서 사용됩니다.

## Hash 알고리즘 활용

[ArrayList](ArrayList.md) 에서 특정한 자료를 찾기 위해서는 모든 데이터를 순회하면서 검색을 해야만 한다. 가령 100만건의 데이터 에서 특정한 데이터를 찾아야 한다면 100만을 전부 순회해야만 하는 상황이 발생한다. 이처럼 **O(N)** 의 속도가 걸리는 자료탐색 을 **O(1)** 로 바꾸는 방법이 있다. 바로 Hash 알고리즘을 활용한 방법이다.

배열 의 특징중 하나가 바로 index 번호를 통해 자료를 O(1) 로 가져올수 있다는 점이다. 

바로 데이터 자체를 인덱스에 맞춰서 넣는 방법이다.가령 `1 ,2 ,5, 8 ,10` 이라는 데이터가 존재한다고 가정 할 경우 이를 인덱스에 활용한다면 다음과 같은 코드로 만들어지게 된다.

총 10 개의 배열 크기를 확보한뒤 각 값 과 일치하는 인덱스에 값을 넣게 되면 우리는 값을 찾기위해 값 자체를 활용하여 손쉽게 값을 찾을수 있다.

```java
public class HashStart2 {  
    public static void main(String[] args) {  
        // 입력  1 2 5 8  
        // [null, 1, 2, null, null, 5, null, null, 8, null]        
        Integer[] arr = new Integer[10];  
        arr[1] = 1;  
        arr[2] = 2;  
        arr[5] = 5;  
        arr[8] = 8;  
        System.out.println("arr = " + Arrays.toString(arr));  
        int searchValue = 8;  
  
        Integer result = arr[searchValue]; // O (1)  
        System.out.println("result = " + result);  
    }  
}
```

하지만 이 경우 메모리 낭비가 심하게 발생한다. 가령 100 만 이라는 숫자를 넣기 위해서는 배열의 크기 또한 100만 으로 크기를 잡아야 한다. 

이를 위해 우리는 나머지 연산을 활용하여 필요한 배열의 크기를 줄일수 있다. 

$$인덱스 = 값\%배열크기$$

예를 들어:

- `1 % 10 = 1` → `arr[1] = 1`
- `16 % 10 = 6` → `arr[6] = 16`

다음은 이를 구현한 코드입니다:

```java
public class HashStart4 {  
    static final int CAPACITY = 10;  
  
    public static void main(String[] args) {  
        // 입력  1 2 5 8 14 99  
        System.out.println("hashIndex(1) = " + hashIndex(1));  
        System.out.println("hashIndex(2) = " + hashIndex(2));  
        System.out.println("hashIndex(5) = " + hashIndex(5));  
        System.out.println("hashIndex(8) = " + hashIndex(8));  
        System.out.println("hashIndex(14) = " + hashIndex(14));  
        System.out.println("hashIndex(99) = " + hashIndex(99));  
  
        Integer[] inputArray = new Integer[CAPACITY];  
        add(inputArray,1);  
        add(inputArray,2);  
        add(inputArray,5);  
        add(inputArray,8);  
        add(inputArray,14);  
        add(inputArray,99);  
  
        System.out.println("inputArray = " + Arrays.toString(inputArray));  
  
        //검색  
        int searchValue = 14;  
        int hashIndex = hashIndex(searchValue);  
        System.out.println("hashIndex = " + hashIndex);  
        Integer integer = inputArray[hashIndex];  
        System.out.println("integer = " + integer);  
  
  
    }  
  
    static int hashIndex(int value) {  
        return value % CAPACITY;  
    }  
  
    private static void add(Integer[] inputArray, int value) {  
        int hashIndex = hashIndex(value);  
        inputArray[hashIndex] = value;  
    }  
}
```

이렇게 할 경우 우리는 hashIndex 함수를 활용해 index 값만 알면 손쉽게 데이터를 O(1) 로 검색할수 있다.


## 해시충돌 

위 와 같은 HASH 알고리즘을 활용에도 단점이 존재한다. 바로 해시 충돌 이다. 만약 데이터 가 `9` 와 `99` 두개가 존재할 경우 나머지는 둘다 `9` 이다. 즉 9번 인덱스에 두개의 값이 들어가게 되고 결국 이는 충돌 이 발생하는 것이다.
이를 해결하는 가장 손쉬운 방법은 바로 배열의 크기를 늘리면 된다. 하지만 이 것은 처음에도 얘기했던 메모리의 낭비 이다. 이는 근본적인 해결책이 아니기 때문에 다른 방법을 찾아야 한다.

### 충돌 해결: 체이닝(Chaining)

충돌 문제를 해결하는 한 방법은 배열의 각 인덱스에 **LinkedList**를 활용하는 것입니다. 충돌이 발생하면 해당 인덱스에 여러 데이터를 저장할 수 있습니다.

예를 들어 충돌이 발생한 경우 9 번 인덱스에 9 와 99 둘다 LinkedList 형태로 들어가있게 된다.

- `[[], [1], [2], [], [], [5], [], [], [8], [99, 9]]`

여기에 데이터 `29`를 추가하면:

- `[[], [1], [2], [], [], [5], [], [], [8], [99, 9, 29]]` 

와 같이 총 3개의 데이터가 9번인덱스 동시에 존재하게 된다. 이후 같은 값을 찾기 위해 9번 인덱스 에서 데이터의 갯수 만큼 순회하여 값을 찾게 된다.  

해시 충돌의 경우 발생 확률이 낮기 때문에 내부에서 아주 작은 반복 횟수만 돌면 된다.

다음은 이를 구현한 코드입니다:

```java
public class HashStart5 {  
    static final int CAPACITY = 10;  
  
    public static void main(String[] args) {  
        // 입력  1 2 5 8 14 9 99  
        LinkedList<Integer>[] buckets = new LinkedList[CAPACITY];  
        System.out.println(Arrays.toString(buckets));  
        for (int i = 0; i < CAPACITY; i++) {  
            buckets[i] = new LinkedList<>();  
        }  
        add(buckets,1);  
        add(buckets,2);  
        add(buckets,5);  
        add(buckets,8);  
        add(buckets,14);  
        add(buckets,99);  
        add(buckets,9);  
  
        System.out.println(Arrays.toString(buckets));  
  
        // 검색  
        int searcheValue = 100 ;  
        boolean contains = contains(buckets, searcheValue);  
        System.out.println(contains);  
  
    }  
  
    private static boolean contains(LinkedList<Integer>[] buckets, int value) {  
        int hashIndex = hashIndex(value);  
        LinkedList<Integer> bucket = buckets[hashIndex]; // LinkedList 형태의 배열  
        return bucket.contains(value);  
    }  
  
    static int hashIndex(int value) {  
        return value % CAPACITY;  
    }  
  
    private static void add(LinkedList<Integer>[] buckets, int value) {  
        int hashIndex = hashIndex(value);  
        LinkedList<Integer> bucket = buckets[hashIndex];  
        if(!bucket.contains(value)) {  
            bucket.add(value);  
        }  
    }  
}
```


# 문자형 및 데이터 의 Hash 

위 예시는 숫자 형 데이터 들을 활용하여 Hash 값을 구하여 이를 Index 에 활용했다. 그렇다면 문자형 과 일반 데이터 자료 형 의 Hash 값은 어떻게 구할까

## 문자형 

문자의 경우 아스키 코드로 하여 각 문자 별 고유의 코드가 존재한다. 컴퓨터의 경우 `1` ,`0` 이 외 의 데이터는 인식하지 못하기 때문에 문자를 표현하기 위해 `1`, `0`  의 조합 으로 문자를 만들게 되고 이를 10 진법으로 표현하면 다음과 같이 된다.

![image.png](https://file-api.ksq9511.synology.me:5353/obsidian-image/20250101223441.png)

가령 다음과 같이 `SET` 라는 문자열은 문자(char) 의 배열 형태이다. `char` 형태의 데이터는 `int` 로 케스팅 이 가능하며 이를 출력 하면 다음과 같이  83 , 69 , 84 로 위 아스키 코드 와 동일하게 출력 이 된다. 

```java
String string = "SET";  
char[] chars = string.toCharArray();  
for (int i = 0; i < chars.length; i++) {  
    System.out.println((int)chars[i]);  
}
```


> [!note] Result
> 83
> 69
> 84

그리고 이 값 들을 바로 Hash 알고리즘에 활용하여 index 값에 쓰게 된다.


```java
public class MyhashSetV2Main {  
    public static void main(String[] args) {  
        MyhashSetV2 myhashSet = new MyhashSetV2(10);  
  
        myhashSet.add("A");  
        myhashSet.add("B");  
        myhashSet.add("C");  
        myhashSet.add("D");  
        myhashSet.add("AB");  
        myhashSet.add("SET");  
          
        System.out.println("myhashSet = " + myhashSet);  
        // 검색  
        String searchValue = "SET";  
        boolean contains = myhashSet.contains(searchValue);  
        System.out.println("contains = " + contains);  
  
    }  
}
```


> [!note] Result
> myhashSet = MyhashSetV2{buckets=[ ](.md), size=6, capacity=10}
> contains = true


## 데이터 

일반적인 사용자 가 만든 객체 의 경우에는 객체의 최상의 클래스인 `Object` 객체의 `hashCode()` 메소드를 통해서 객체의 Hash 값을 구할수 있다. 

### Object.hashCode(); Object.equals

만약 사용하는 객체에서 `hashCode()` 를 오버라이딩 하지 않고 사용할 경우 이는 객체의 참조값 을 기반으로 하여 해시 코드를 생성하게 된다. 즉 `new` 키워드를 사용하여 만든 객체는 안의 데이터가 같아도 해시 코드가 다르다.

```java
public class JavaHashCodeMain {  
    public static void main(String[] args) {  
        Member idA = new Member("idA");  
        Member idB = new Member("idA");  
  
        System.out.println("idA.hashCode() = " + idA.hashCode());  
        System.out.println("idB.hashCode() = " + idB.hashCode());  
        System.out.println("idA.equals(idB) = " + idA.equals(idB));  
        System.out.println("(idA == idB) = " + (idA == idB));  
    }  
}
```


아래의 결과에 보다 시피 `idA` 객 체와 `idB` 은 서로 같은 값을 사용하여 객체를 생성하였으나 결과에서는 둘이 전혀 다른 해쉬 값을 가져오는 것 을 알수 있다 . 그렇기 때문에 Hash 알고리즘을 활용한 [Collection Framework](Collection%20Framework.md) 에서는 서로 다른 객체로 인식 하고 중복 또한 제거할수 없다

> [!note] Result
> idA.hashCode() = 122883338
> idB.hashCode() = 666641942
> idA.equals(idB) = false
> (idA == idB) = false


그렇기 때문에 아래와 같이 `Object` 객체의 `equals` , 와 `hashCode` 를 [오버라이딩](%EC%98%A4%EB%B2%84%EB%9D%BC%EC%9D%B4%EB%94%A9(Overriding)%20%EA%B3%BC%20%EC%98%A4%EB%B2%84%EB%A1%9C%EB%94%A9(Overloading).md) 하여 직접 구현을 해줘야 한다. 

아래의 코드는 Member 객체의 `id` 를 활용하여 해쉬 코드 를 만들고 member 객체의 `id` 일치 여부를 판별한다. 

```java
  
@Override  
public boolean equals(Object object) {  
    if (object == null || getClass() != object.getClass()) return false;  
    Member member = (Member) object;  
    return Objects.equals(id, member.id);  
}  
  
@Override  
public int hashCode() {  
    return Objects.hash(id);  
}
```

* equals 오버라이딩 누락 : 값의 비교가 불가능 하기 때문에 Hash 알고리즘을 활용하여 데이터를 저장하여도 같은 값을 찾을수 없음
* hashCode 오버라이딩 누락 : 같은 객체의 중복 제거를 할수 없기 때문에 해쉬 알고리즘을 활용한 자료구조에서 잘못된 위치에 데이터 가 저장됨 


# 결론

자바에서 hash 는 상당히 많은 곳 에서 활용하여 사용된다. 특히 아래의 컬렉션 프레임 워크 의 핵심 기술이다. 

* [SET](Set.md)
* [LinkedHashSet](LinkedHashSet.md)
* [TreeSet](TreeSet.md)


--- 

# 참고

https://www.inflearn.com/course/%EA%B9%80%EC%98%81%ED%95%9C%EC%9D%98-%EC%8B%A4%EC%A0%84-%EC%9E%90%EB%B0%94-%EC%A4%91%EA%B8%89-2/dashboard
