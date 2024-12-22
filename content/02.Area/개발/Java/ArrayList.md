---
date: 2024 년 12 월 19 일 10 시 12 분
tags:
  - Dev
  - ArrayList
  - CollectionFrameWork
author: Joung Dong Hee
share: true
---

# ArrayList

`ArrayList`는 자바의 [Collection Framework](Collection%20Framework.md) 중 하나로, 기존 배열 형태의 구조에 여러 기능을 추가한 동적 배열 자료 구조입니다.

## 기존 배열 의 문제

기존 배열은 다음과 같은 제약을 가지고 있습니다:

1. **크기 고정**: 배열은 초기 선언 시 크기가 고정되며, 이후 크기를 동적으로 변경할 수 없습니다.
    - 크기가 부족하면 `ArrayIndexOutOfBoundsException`이 발생합니다.
    - 크기를 너무 크게 설정하면 메모리 낭비가 발생할 수 있습니다.
2. **삽입 및 삭제 비효율성**: 배열 중간에 요소를 삽입하거나 삭제하려면, 기존 요소들을 이동해야 하므로 성능 저하가 발생합니다.

이러한 문제를 해결하기 위해 `ArrayList`가 도입되었습니다.

```java
int[] ints = new int[5];
ints[0] = 1;
ints[1] = 2;
ints[3] = 4;  // 잘못된 인덱스 사용 예시
ints[4] = 5;
ints[5] = 6;  // ArrayIndexOutOfBoundsException 발생
```


## ArrayList 

`ArrayList`는 다음과 같은 특징을 갖습니다:

1. **동적 크기**: 초기 크기를 지정하지 않아도 내부적으로 크기를 자동으로 조정합니다.
2. **배열 기반 구현**: 내부적으로 배열을 사용하여 데이터를 저장하되, 크기가 부족할 경우 자동으로 배열을 확장합니다.

```java
// 크기 미 지정시 
ArrayList<String> arrays = new ArrayList<>(); 
// 크기를 지정 할 경우 
ArrayList<String> arrays5 = new ArrayList<>(5);
```

### ArrayList의 내부 동작: `add`와 `grow`

`ArrayList`의 `add` 메서드는 배열의 길이와 요소의 개수가 같아질 경우, 내부적으로 `grow` 메서드를 호출하여 배열 크기를 확장합니다.

### ArrayList.add

^ffc47a

```java
private void add(E e, Object[] elementData, int s) {  
    if (s == elementData.length)  
        elementData = grow();  
    elementData[s] = e;  
    size = s + 1;  
}
```


배열 크기를 증가시키는 로직은 다음과 같습니다:
- 기본적으로 기존 크기의 **1.5배**로 확장합니다.
- 새 배열을 생성하고 기존 데이터를 복사합니다.

```java
private Object[] grow(int minCapacity) {  
    int oldCapacity = elementData.length;  
    if (oldCapacity > 0 || elementData != DEFAULTCAPACITY_EMPTY_ELEMENTDATA) {  
        int newCapacity = ArraysSupport.newLength(oldCapacity,  
                minCapacity - oldCapacity, /* minimum growth */  
                oldCapacity >> 1           /* preferred growth */);  
        return elementData = Arrays.copyOf(elementData, newCapacity);  
    } else {  
        return elementData = new Object[Math.max(DEFAULT_CAPACITY, minCapacity)];  
    }  
}
```

이처럼 ArrayList 는 기존 배열 처럼 크기가 정적 이지 않고 매번 배열의 크기를 비교해 배열이 전부다 찰 경우 크기를 증가시킨 새로운 배열이 만들어지게 된다. 


### ArrayList 의 제네릭 

ArrayList 에서는 [제네릭 Generic](%EC%A0%9C%EB%84%A4%EB%A6%AD%20Generic.md) 을 선언함으로서 타입의 안전성을 제공합니다. 이를 통해 컴파일 시점에 타입 체크가 가능하며, 선언한 타입 이외의 객체는 저장할 수 없습니다.

```java
public class ArrayList<E> extends AbstractList<E>  
        implements List<E>, RandomAccess, Cloneable, java.io.Serializable {
    // ...
}

```



## ArrayList 단점

1. **탐색 속도**:  특정 값을 찾으려면 모든 요소를 순회해야 하므로, 탐색 시간 복잡도는 [O(N)](Big%20O%20%ED%91%9C%EA%B8%B0%EB%B2%95.md#^bc3131) 입니다.
2. **잦은 배열 복사**:  배열이 가득 찼을 때 크기를 증가시키면서 `Arrays.copyOf`를 사용해 데이터를 복사하므로 성능 저하가 발생할 수 있습니다. ([ ArrayList.add](ArrayList.md#^ffc47a))
3. **비효율적인 삽입/삭제**: 중간에 데이터를 삽입하거나 삭제할 경우, 뒤쪽 데이터를 이동해야 하므로 성능이 떨어질 수 있습니다.


## ArrayList 장점

1.  **빠른 임의 접근**: `index`를 이용한 접근 시간 복잡도는 [O(1)](Big%20O%20%ED%91%9C%EA%B8%B0%EB%B2%95.md#^1244c4) 입니다.
2.  **순서 보장 및 중복 허용**: 입력 순서가 유지되며, 동일한 값을 중복해서 저장할 수 있습니다.
3.  **유연한 크기 조정**: 크기가 자동으로 조정되므로 메모리 관리가 편리합니다.

## ArrayList 용도

- **데이터 크기가 자주 변경되지 않는 경우**에 적합합니다.  
    예: 고정된 크기의 목록, 순차적인 데이터 처리.
- 탐색 속도보다 삽입/삭제 빈도가 낮은 경우에 유리합니다.


## **LinkedList 와의 비교**
- [ArrayList](ArrayList.md) 는 임의 접근이 빠르지만, 삽입/삭제가 느립니다.
- [LinkedList](LinkedList.md) 는 삽입/삭제가 빠르지만, 임의 접근이 느립니다.