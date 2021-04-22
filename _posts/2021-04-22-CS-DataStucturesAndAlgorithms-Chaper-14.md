---
layout: post
title: "Data Structures & Algorithms Chapter 14: Binary Search Trees"
date: 2021-04-22
category: read 
excerpt: ""


---

# [CS-Data Structures & Algorithms] Chapter 14: Binary Search Trees

- Binary search tree(BST / 이진 탐색 트리)는 빠른 탐색, 추가, 제거 기능을 하는 자료 구조이다.  

- 이진 탐색 트리는 이전 챕터에서 살펴본 Binary Tree(이진 트리)에서 두가지 룰이 추가된다.  
    - Left child가 가지는 값은 parent보다 작아야 한다.  
    - 따라서, right child가 가지는 값은 parent보다 크거나 같아야 한다.  
    이진 탐색 트리는 위 룰을 통해 불필요한 확인을 하지 않도록 도와준다.  
- 이진 탐색 트리는 탐색, 추가, 제거에 대한 시간 복잡도로 O(log n)을 가진다.  
    (이는 Array나 Linked list와 같은 선형의 (O(n)의 시간 복잡도를 가지는)자료구조보다 빠르다.)  
    - 예를 들어 탐색으로 비교해보면, Array는 Array 내부의 각 요소를 확인하고 찾고자 하는 값을 찾지만,  
        이진 탐색 트리는 요소를 확인할 때 마다 선택지를 반으로 줄인다.

## Implementation

---

### Binary Search Tree

- 이진 탐색 트리를 구현하는 방법은 우선, 제네릭 타입의 BinarySearchTree 구조체를 선언하고 프로퍼티로 BinaryNode 타입의 root를 정의한다.  
    (이진 탐색 트리가 받는 요소(Element)는 Comparable 프로토콜을 준수해야 합니다.(비교가 가능해야 함))

### Inserting elements

- Element 타입을 매개변수로 받는 mutating 메서드로 insert를 하나 정의하고,  
    - 이 메서드는 root 노드를 정의해주기 위해 정의하는 메서드로,  
        메서드 내부에서 아래 메서드를 호출하며 매개변수로 root 노드와 전달인자로 받은 Element 타입의 값을 준다.

- BinaryNode 타입과 Element 타입의 값을 매개변수로 받아서 BinaryNode 타입을 반환하는 insert 메서드를 정의한다.  
    - 내부에서 매개변수로 받는 BinaryNode 타입을 옵셔널 바인딩 해준다.  
        (노드가 없는 공간에 Insert를 할 경우, 처음 값은 nil이기 때문에  
        그리고 node가 없는 공간에 도착 했다는 것은 목표에 도착했다는 말이기 때문에, BinaryNode를 반환하고 재귀를 끝낸다.)  
    - if - else 문을 통해, 추가한 값(Element 타입)이 node보다 작으면 왼쪽, 아니라면 오른쪽 자식 노드를 매개변수로 insert 메서드를 부른다.(재귀적)  
    - 모든 동작이 끝난 후 node를 반환한다.  
        (새로운 노드를 반환하거나, leftChild 또는 rightChild가 추가된 노드를 반환한다.)

### Finding elements

- 요소를 찾는 것은 이진 트리에서 구현한 In-Order Traverse를 통해서도 할 수 있지만,  
    이진 탐색 트리를 사용하면 더 효과적으로 구현할 수 있다.  

- 매개변수로 받는 값을 이진 탐색 트리가 가지고 있는지 확인한 후 True or False로 반환하는 메서드를 구현하면  
    - root node값을 메서드 안의 변수(current라고 하자)에 넣은 후  
    - while문을 통해서 current 변수의 값을 가지는 node 상수를 선언하고  
    - while문 내부에서 node의 값이 전달인자로 주어진 값과 같으면 true를 반환하고  
    - 다르면 if - else 문을 통해 찾는 값이 node의 값보다 작으면 left, 아니라면 rightChild의 값을 current변수에 준다.  
    - 위의 내용으로 while문 내부에서 반복하다가 찾으면 세번째에서 기재한 내용이 실행되고,  
        못 찾으면(node.left or rightChild가 nil일 때) false를 반환한다.

### Removing elements

- 이진 탐색 트리에서 요소를 제거하는 경우, 세가지 케이스가 있는데  
    - Leaf: Leaf 노드를 제거하는 케이스로 간단하게 마지막 노드만 제거하면 된다.  
    - Nodes with one child: 제거하는 노드가 자식 노드를 한 개 가진 케이스로, 기존 노드의 부모 노드와 연결시킨 후 기존 노드는 제거한다.  
    - Nodes with two children: 제거하는 노드가 2개의 자식 노드를 가진 케이스로 rightChild 노드의 가장 왼쪽 아래 노드를 기존 노드를 제거한 후 대체한다.  
        (부모 노드보다 작은 노드는 왼쪽으로, 크거나 같은 노드는 오른쪽으로 보내는 매커니즘을 유지하려면 오른쪽 노드 중 가장 작은 노드(오른쪽 자식 노드보다 작고, 왼쪽 자식 노드보다 크다.)로 대체해야 한다.)

- 위 케이스들을 고려해서 아래와 같이 구현한다.  
    - 우선 BinaryNode에 min 프로퍼티를 추가하고 min은 아래와 같이 선언한다.  

        ```swift
        var min: BinaryNode {
        		// leftChild가 있다면, leftChild의 min 값을 받고
            // 없다면 자기 자신을 반환한다. = 왼쪽 제일 아래 노드를 가진다.
            leftChild?.min ?? self
        }
        ```

    - 그 후 이진 탐색 트리 구조체에 BinaryNode와 Element 타입을 매개변수로 받아서 BinaryNode 타입을 반환하는 remove 메서드를 선언한다.  
    - 해당 매서드 내부에 전달인자로 받은 노드를 옵셔널 바인딩한다.(노드가 nil이라면 nil을 반환한다.)  
    - 그 후 제거할 값이 받은 노드의 값과 같다면 아래 분기를 거친다.  
        1. node의 left / rightChild 모두 nil인 경우, return nil(자식이 없는 경우)  
        2. leftChild가 nil인 경우, node의 rightChild를 반환한다.(자식이 하나인 경우)  
        3. rightChild가 nil인 경우, node의 leftChild를 반환한다.(자식이 하나인 경우)  
        4. 위 세가지 if문을 통과한 후에도 살아있다면(자식이 둘인 경우)  
            - node의 값을 node의 rightChild의 min의 값으로 바꾼다.  
            - rightChild에 있는 바뀐 노드 값과 같은 값을 가진 노드를 지운다.(remove 메서드)  
    - 값이 같지 않다면, else if 문으로 값이 노드의 값보다 큰 지 작은 지 확인한다.  
    - 작다면 leftChild를, 크다면 rightChild에 remove 메서드를 재귀적으로 호출한 후 반환 값을 준다.  

## Key points of the chapter

---

- 이진 탐색 트리는 정렬된 데이터를 유지하기 위한 강력한 자료구조이다.  
- 이진 탐색 트리의 요소는 반드시 Comparable 해야 한다.  
- 이진 탐색 트리의 Insert, remove, contains 메서드의 시간 복잡도는 O(log n)이다.  
- 트리가 언밸런스하게 된다면 성능은 O(n)으로 하향될 것이다.  
    챕터 16에서 배우는 AVL(self-balancing binary search tree)에서 이를 방지할 수 있다.
