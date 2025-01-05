---
date: 2025 년 01 월 06 일 00 시 01 분
tags:
  - Dev
  - Deque
  - stack
  - queu
  - offer
  - pop
  - poll
  - push
author: Joung Dong Hee
share: true
---

# Deque

`Deque`는 **Double Ended Queue**의 약자로, 양방향에서 데이터를 삽입 및 제거할 수 있는 유연한 자료구조입니다.  
자바에서는 `Deque` 인터페이스와 이를 구현한 클래스(`ArrayDeque`, `LinkedList` 등)를 통해 [Stack 스택](Stack%20%EC%8A%A4%ED%83%9D.md)과 [Queu 큐](Queu%20%ED%81%90.md)의 기능을 통합하여 제공합니다.

Deque는 양쪽 끝에서 작업을 수행할 수 있기 때문에 `Stack`과 `Queue`의 기능을 모두 지원합니다. 이로 인해 대기열 및 역추적과 같은 다양한 시나리오에서 사용됩니다.


![image.png](https://file-api.ksq9511.synology.me:5353/obsidian-image/20250106002657.png)


## 주요 메서드 

Deque 는 `Stack` 과 `Queue` 의 기능 을 모두 가지고 있기 때문에 동일한 메서드를 사용할수 있다.

### 데이터 삽입

- `offerFirst(E e)` : 앞쪽에 데이터를 추가합니다.
- `offerLast(E e)` : 뒤쪽에 데이터를 추가합니다.

### 데이터 조회

- `peekFirst()` : 앞쪽에서 데이터를 조회(제거하지 않음).
- `peekLast()` : 뒤쪽에서 데이터를 조회(제거하지 않음).

### 데이터 제거

- `pollFirst()` : 앞쪽에서 데이터를 꺼내고 제거.
- `pollLast()` : 뒤쪽에서 데이터를 꺼내고 제거.


> [!info] 참고
> `Deque`는 `push`(스택 기능)과 `pop`(스택의 데이터 제거) 메서드도 제공합니다.


## Example Java 

```java
public class DequeMain {  
    public static void main(String[] args) {  
        Deque<Integer> deque = new ArrayDeque<>();  
  
        // 데이터 추가  
        deque.offerFirst(1);  
        System.out.println("deque = " + deque);  
        deque.offerFirst(2);  
        System.out.println("deque = " + deque);  
        deque.offerLast(3);  
        System.out.println("deque = " + deque);  
        deque.offerLast(4);  
        System.out.println("deque = " + deque);  
        // 데이터를 꺼내지 않고 조회만  
        System.out.println("deque.peekFirst() = " + deque.peekFirst());  
        System.out.println("deque.peekLast() = " + deque.peekLast());  
        // 데이터 꺼내기  
        System.out.println("deque.pollFirst() = " + deque.pollFirst());  
        System.out.println("deque.pollFirst() = " + deque.pollFirst());  
        System.out.println("deque.pollFirst() = " + deque.pollLast());  
        System.out.println("deque.pollFirst() = " + deque.pollLast());  
        System.out.println("deque = " + deque);  
  
    }  
}
```


# Deque를 Queue로 사용하기

`Deque` 를 사용한 `Queue` 자료형 

```java
public class DequeQueueMain {  
    public static void main(String[] args) {  
        Deque<Integer> deque = new ArrayDeque<>();  
        // 데이터 추가  
        deque.offer(1);  
        deque.offer(2);  
        deque.offer(3);  
        System.out.println("deque = " + deque);  
  
        // 다음 꺼낼 데이터르 확인 꺼내지 않고 단순 조회만  
        System.out.println("deque.peek() = " + deque.peek());  
  
        // 데이터 꺼내기  
        System.out.println("deque.pop() = " + deque.poll());  
        System.out.println("deque.pop() = " + deque.poll());  
        System.out.println("deque.pop() = " + deque.poll());  
        System.out.println("deque = " + deque);  
    }  
}
```


# Deque를 Stack으로 사용하기

`Deque` 자료형을 사용한 `stack`

```java
public class DequeStackMain {  
    public static void main(String[] args) {  
        Deque<Integer> deque = new ArrayDeque<>();  
  
        // 데이터 추가  
        deque.push(1);  
        deque.push(2);  
        deque.push(3);  
        System.out.println("deque = " + deque);  
  
        // 다음 꺼낼 데이터르 확인 꺼내지 않고 단순 조회만  
        System.out.println("deque.peek() = " + deque.peek());  
  
        // 데이터 꺼내기  
        System.out.println("deque.pop() = " + deque.pop());  
        System.out.println("deque.pop() = " + deque.pop());  
        System.out.println("deque.pop() = " + deque.pop());  
        System.out.println("deque = " + deque);  
    }  
}
```