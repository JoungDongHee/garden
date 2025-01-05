---
date: 2025 년 01 월 05 일 20 시 01 분
tags:
  - Dev
  - stack
  - push
  - pop
author: Joung Dong Hee
share: true
---

# Stack 스택

스택은 가장 흔히 사용되는 자료구조 중 하나로, 대부분의 프로그래밍 언어에서 제공하거나 구현 가능합니다.

스택은 이름 그대로 물건을 쌓는 "스택(stack)" 처럼 데이터를 쌓아 올리는 형태를 나타냅니다.

## Push (삽입)

데이터를 스택에 추가하는 연산입니다. 데이터는 스택의 "맨 위(top)"에 추가됩니다.

![image.png](https://file-api.ksq9511.synology.me:5353/obsidian-image/20250105204303.png)



가령 위 이미지 와 같이 총 4개의 데이터 를 넣기 위해서는 10  -> 20 -> 30 -> 40 순으로 아래에서 부터 차례대로 쌓아 올려야만 한다. (Push)

## Pop (제거)

데이터를 스택에서 제거하는 연산입니다. 데이터는 스택의 "맨 위(top)"에서 제거됩니다.

![image.png](https://file-api.ksq9511.synology.me:5353/obsidian-image/20250105205016.png)

데이터를  꺼내기 위해서는 위에서 부터 40 -> 30 -> 20 -> 10 순으로 입력한 순서의 정 반대로 나오게 된다. 

이를 해석하면 나중에 들어온 데이터가 제일 먼저 나간다 라고 하여 한국어로 *후입선출*  혹은 영어로 *LIFO(Last In First Out)*  라고 한다.


참고로 stack 자료구조의 시간복잡도는 전부 O(1) 이다.

## Example Java

Java에서는 `Stack` 클래스를 제공하여 스택 자료구조를 사용할 수 있습니다. 

하지만, `Stack` 클래스는 **`Vector`를 기반으로 구현**되었기 때문에, 최신 Java 애플리케이션에서는 **`Deque` 인터페이스**를 사용하는 것이 권장됩니다.

**Deque와 Stack 비교**

- **Stack**: 레거시 클래스이며, `Vector`를 상속받아 동기화를 지원합니다. 하지만 성능 상의 이유로 잘 사용되지 않습니다.
- **Deque**: 더 간단하고 빠른 대안으로, `ArrayDeque`나 `LinkedList`를 통해 구현할 수 있습니다. 동기화가 필요 없는 경우 더 적합합니다.


```java
import java.util.Stack;

public class StackMain {
    public static void main(String[] args) {
        Stack<Integer> stack = new Stack<>();

        // 데이터 삽입
        stack.push(10);
        stack.push(20);
        stack.push(30);

        // 최상단 데이터 확인
        System.out.println("Top element: " + stack.peek()); // 30

        // 데이터 제거
        System.out.println("Pop: " + stack.pop()); // 30
        System.out.println("Pop: " + stack.pop()); // 20
        System.out.println("Pop: " + stack.pop()); // 10

        // 스택이 비었는지 확인
        System.out.println("Is stack empty? " + stack.isEmpty());
    }
}

```

---

# 참고 

https://www.geeksforgeeks.org/introduction-to-stack-data-structure-and-algorithm-tutorials/