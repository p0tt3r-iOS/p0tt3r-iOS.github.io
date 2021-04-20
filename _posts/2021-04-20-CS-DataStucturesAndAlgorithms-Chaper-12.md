---
layout: post
title: "Data Structures & Algorithms Chapter 12: Binary Trees"
date: 2021-04-20
category: read 
excerpt: ""


---

# [CS-Data Structures & Algorithms] Chapter 12: Binary Trees

- Binary Tree란 최대 두 개의 자식(child) 노드을 가지는 Tree이다.(주로 left, right child로 나눈다.)  
- Binary Tree는 많은 Tree 자료구조와 알고리즘에 사용된다.

## Implementation

---

### Binary Node

- 값, 왼쪽 자식 노드, 오른쪽 자식 노드를 정의한다.  
- 값을 받아서 생성하는 이니셜라이저를 정의한다.

## Traversal algorithms(순회 알고리즘)

---

### In-order traversal

- Root 노드에서 시작해서 순서대로 주어진 명령을 수행합니다.  
- 현재 노드가 왼쪽 자식 노드를 가지고 있다면, 재귀적으로 해당 노드를 먼저 확인합니다.  
- 그 후 자신을 방문하고,  
- 오른쪽 자식 노드를 가지고 있다면, 재귀적으로 해당 노드를 확인합니다.  
- In-oder traversal은 오름차순으로 노드를 확인합니다.

### Pre-order traversal

- Root 노드에서 출발해서 현재 노드를 먼저 확인합니다.  
- 그 후 왼쪽 노드를 가지고 있다면, 재귀적으로 해당 노드를 확인합니다.  
- 마지막으로 오른쪽 노드를 가지고 있다면, 해당 노드를 확인합니다.

### Post-order traversal

- Root 노드에서 출발해서 왼쪽 노드가 있다면, 재귀적으로 해당 노드를 확인합니다.  
- 그 후 오른쪽 노드를 가지고 있다면, 재귀적으로 해당 노드를 확인합니다.  
- 마지막으로 현재 노드 확인합니다.

## Example

![https://user-images.githubusercontent.com/46529663/115379278-56881c00-a20c-11eb-922b-2afdab33cd9f.png](https://user-images.githubusercontent.com/46529663/115379278-56881c00-a20c-11eb-922b-2afdab33cd9f.png)

위 Binary Tree를 기준으로 각 순회 알고리즘에 print()를 매개변수로 출력해보면,  

- In-order traversal: 1 → 3 → 5 → 9 → 5  
- Pre-order traversal: 9 → 3 → 1 → 5 → 5  
- Post-order traversal: 1 → 5 → 3 → 5 → 9

## Key points of the chapter

---

- 이진 트리는 일부 가장 중요한 트리 구조의 기반이다.  
    이진 탐색 트리와 AVL 트리는 삽입 / 삭제 동작을 제한하는 이진 트리이다.

- 위에서 살펴본 세가지 순회 알고리즘은 이진 트리에서만 중요한 것이 아니다.  
    다른 종류의 트리에서 데이터를 사용할 때에도, 위 알고리즘을 주로 사용한다.
