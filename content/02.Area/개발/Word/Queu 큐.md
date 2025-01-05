---
date: 2025 년 01 월 05 일 21 시 01 분
tags:
  - Dev
  - queu
  - 큐
  - offer
  - poll
author: Joung Dong Hee
share: true
---

# Queu 큐

큐는 [Stack 스택](Stack%20%EC%8A%A4%ED%83%9D.md) 과 함께 가장 흔히 사용되는 자료구조 중 하나입니다. 스택이 **후입선출(Last In, First Out; LIFO)** 특성을 가진다면, 큐는 **선입선출(First In, First Out; FIFO)** 특성을 갖습니다.

쉽게 말해, 큐는 사람들이 줄을 서는 대기열과 같은 구조입니다. 먼저 줄을 선 사람이 먼저 처리되는 방식입니다.

![image.png](https://file-api.ksq9511.synology.me:5353/obsidian-image/20250105210887.png)


## offer

큐에 데이터를 추가하는 작업으로, 데이터를 큐의 "뒤쪽(rear)"에 삽입합니다.

## poll 

큐에서 데이터를 제거하는 작업으로, 데이터를 큐의 "앞쪽(front)"에서 추출합니다.


![image.png](https://file-api.ksq9511.synology.me:5353/obsidian-image/20250105211695.png)



## Example Java

Java 에서는 `Queue` 클래스를 제공하여 이를 통해 큐 형태의 자료구조를 사용할수 있다.

```java
import java.util.Queue;
import java.util.ArrayDeque;

public class QueueMain {
    public static void main(String[] args) {
        Queue<Integer> queue = new ArrayDeque<>();

        // 데이터 추가
        queue.offer(10);
        queue.offer(20);
        queue.offer(30);

        System.out.println("Queue: " + queue); // [10, 20, 30]

        // 가장 앞의 데이터 확인
        System.out.println("Peek: " + queue.peek()); // 10

        // 데이터 추출
        System.out.println("Poll: " + queue.poll()); // 10
        System.out.println("Poll: " + queue.poll()); // 20
        System.out.println("Poll: " + queue.poll()); // 30

        // 큐가 비었는지 확인
        System.out.println("Is queue empty? " + queue.isEmpty()); // true
    }
}

```


---

# 참고

https://www.geeksforgeeks.org/introduction-to-queue-data-structure-and-algorithm-tutorials/