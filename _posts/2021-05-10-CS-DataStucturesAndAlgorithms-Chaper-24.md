---
layout: post
title: "Data Structures & Algorithms Chapter 24: Priority Queue"
date: 2021-05-10
category: read 
excerpt: ""


---

# [CS-Data Structures & Algorithms] Chapter 24: Priority Queue

- 큐는 FIFO를 이용하여 요소들의 순서를 유지하는 간단한 배열이다.  
- Priority Queue(우선순위 큐)는 큐의 다른 버전으로 FIFO를 사용하는 대신 아래 두가지 방식 중 하나로 구성된다.  
    1. Max-priority: 맨 앞의 요소는 언제나 가장 큰 값이다.  
    2. Max-priority: 맨 앞의 요소는 언제나 가장 작은 값이다.

## Applications

---

우선순위 큐는 몇가지 방식을 적용하는데,  
- 다익스트라 알고리즘: 우선순위 큐에서 가장 작은 비용을 소모하도록 한다.(최단 경로를 찾는다.)  
- A* 알고리즘: 우선순위 큐에서 탐색되지 않은 경로를 최단 경로 생성을 위해 추적한다.  
- Heap 정렬: 우선순위 큐를 사용하여 구현할 수 있다.  
- 허프만 코딩: 압축 트리를 구축한다.   
(min-priority queue에서 아직 부모 노드가 없는 두 노드를 가장 짧은 빈도로 찾기 위해 사용한다.)

## Common operations

---

Chapter 8에서 구현한 Queue를 구현한다.

## Implementation

---

아래와 같은 방법으로 우선순위 큐를 구현한다.  
1. Sorted array(정렬된 배열): 최대 값 또는 최소 값을 얻는데 유용하고 O(1) 시간 복잡도를 가진다.  
하지만, 삽입은 순서대로 삽입해야 하므로 O(n)의 시간 복잡도를 가진다.  
2. Balanced binary search tree(균형 이진 탐색 트리):  Deque을 생성할 때, 유용하며  
최소 값과 최대 값을 얻을 때, O(log n)의 시간 복잡도를 가진다.  
삽입 또한 Sorted array보다 빠른 시간 복잡도인 O(log n)을 가진다.  
3. Heap: 힙은 우선순위 큐를 구현할 때 일반적으로 사용된다.  
힙은 Sorted Array와 달리 부분적 정렬만 필요로 하기 때문에 더 효과적이고,  
min-priority 큐에서 최소 값 추출 / max-priority 큐에서 최대 값 추출(O(1))을 제외한   
모든 Heap의 동작은 O(log n) 시간 복잡도를 가진다.  
  
우선순위 큐를 구현하기 위해 필요한 자료구조를 정의한다.  
1. Heap: 우선순위 큐에 사용될 Heap 자료구조를 구현한다.  
2. Queue: 우선순위 큐에 적용할 큐 프로토콜을 정의한다.  
  
이제 코드로 우선순위 큐를 구현하면,  
1. Equatable을 준수하는 제네릭 타입의 매개변수를 가지며,   
Queue 프로토콜을 준수하는 PriorityQueue 구조체를 정의한다.  
2. 우선순위 큐를 구현하기 위해 내부에 heap을 정의한다.  
3. 정렬 방식과 배열을 매개변수로 받아서 해당 매개변수로 heap을 초기화하는 생성자를 정의한다.  
4. Queue 프로토콜을 준수하기 위해 필요한 메서드(enqueue(_:), dequeue() 등)를 정의해준다.  

## Key points of the chapter

---

- 우선순위 큐는 우선순위에서 요소를 찾아내는 작업에 주로 사용된다.  
- 우선순위 큐를 구현할 때, 큐의 주요 동작에 초점을 맞추고,   
힙 자료구조가 제공하는 기능을 생략하여 추상화 계층을 만든다.  
- 이는 우선순위 큐의 의도를 명확하고 간결하게 한다.  
우선순위 큐는 요소들을 enqueue하고 dequeue하는 작업만 할 뿐이다.  
