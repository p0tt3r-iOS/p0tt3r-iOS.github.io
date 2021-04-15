---
layout: post
title: "Data Structures & Algorithms Chapter 6: Linked List"
date: 2021-04-14
category: read 
excerpt: ""


---

# [CS-Data Structures & Algorithms]  Chapter 6: Linked List

- Linked list는 단일 방향이며 직선의 형태를 가진, 값들의 모음입니다.  
    (글을 잘 못써서 값이 직선의 형태를 가진 것 처럼 보이는데, Linked list가 직선 형태)  
    - 앞에서 부터의 삽입과 제거에 일정한 시간을 가진다.  
    - 신뢰할 수 있는 성능 특성들을 가진다.  
    - Linked List는 노드들이 체인으로 연결된 모습을 띄고 있으며,  
        노드에는 두가지 책임이 있다.  
        1. 값을 유지한다.  
        2. 다음 노드에 대한 참조를 유지한다.  
        (이 참조 값이 nil 이라면 자신이 마지막 노드임을 뜻한다.)

## Node

---

- Node는 Value와 next를 프로퍼티로 가진 제네릭 타입으로 구현한다.  
- next는 Node타입을 받으며 다음 노드를 연결하는 역할을 한다.

## LinkedList

---

- LinkedList는 head와 tail(둘 다 Node 타입)을 프로퍼티로 가진다.  

## Adding values to the list

---

LinkedList를 구현하기 위해서는 Node 객체를 관리하는 인터페이스를 제공해야 하는데,  
먼저, 값을 추가하는 인터페이스를 확인해보자  
(아래 방법들은 각자 자신만의 고유한 성능을 가진다.)  
1. push: 값을 list 앞에 추가한다.  
2. append: 값을 list 마지막에 추가한다.  
3. insert(after:): 값을 리스트의 특정 노드 뒤에 추가한다.  

- push operation(head-first insertion / 시간 복잡도: O(1))  
    - 기존 head를 새로 추가하는 값을 가진 노드의 next에 넣고, head에 새 노드를 넣는다.  
    - 만약 빈 LinkedList라면 tail에도 새 노드를 넣어준다.

- append operation(tail-end insertion / 시간 복잡도: O(1))  
    - 기존 tail의 next에 새 노드를 추가하고, tail을 새 노드로 교체한다.  
    - LinkedList가 비어있다면 새 노드를 push한다.

- insert(after:) operation(시간 복잡도: O(1) / 하지만 index를 찾는 과정은 O(n)  

    위 작업은 두가지 과정을 거치는데, 아래와 같다.  

    1. after: 에서 넣은 index를 가진 특정 노드를 찾는다.  
    2. 찾은 노드 다음에 새 노드를 삽입한다.

    위 과정을 코드로 구현한 방식을 설명해보면,  

    - 노드와 인덱스에 대한 참조 변수를 만든다.  
    - while문을 통해 전달인자로 준 인덱스를 찾을 때까지 인덱스를 증가시키고,  
        노드를 다음 노드로 넘긴다.  
        (list가 비어있거나, Index가 list의 범위를 벗어나면 nil을 반환한다.)

    - 해당 인덱스를 찾았을 때, 해당 인덱스를 가지는 노드가 tail이라면, append()를 하고,  
        아니라면 새로운 노드의 next에 기존 노드 next를 넣고,  
        기존 노드의 next에 새 노드를 넣는다.  
        (사이에 넣으니까, 새 노드의 next는 원래 노드의 다음 노드를 가져야 함)

## Removing values from the list

---

아래는 노드를 제거하는데 사용하는 세가지 주요 작업이다.  

1. pop: list 제일 앞에 있는 값을 제거한다.  
2. removeLast: list 제일 뒤에 있는 값을 제거한다.  
3. remove(at:): 특정 위치의 값을 제거한다.

- pop operation(시간 복잡도: O(1))  
    - defer을 이용해서 head를 head.next로 바꾸고, 만약 list가 비었다면 nil을 반환한다.  
    - head의 value를 반환하고, defer 내의 코드가 실행된다.  
        (리스트가 빈 경우를 대비해 반환 타입은 옵셔널로 지정한다.)  
        defer을 통해 반환 후에 head를 다음 노드로 바꾸면서 기존 헤드 값은 리스트에서 사라진다.

- removeLast operation(시간 복잡도: O(n))  
    - head가 없다면(nil이라면) nil을 반환한다.  
    - head.next가 없다면 pop operation을 사용한다.  
        (head.next가 없으면 head가 마지막 값이라, 삭제하고 nil(next)를 head에 넣는다.  
    - head부터 while문으로 next가 없는 노드를 찾는다.(마지막 요소를 찾기 위해)  
    - 찾으면 while문을 탈출하고, 해당 노드의 이전 노드의 next를 nil로 바꾸고,  
        이전 노드를 tail로 만든다.(여기서 마지막 노드와의 체인이 끊긴다.)  
    - 그리고 찾은 노드의 값을 반환한다.

- remove(after:) operation(시간 복잡도: O(1))
    위 작업의 기본적인 원리는 after:에 특정 노드를 전달인자로 받아서,  
    해당 노드(1번)의 next(2번)를 끊고, 다음 노드(2번)의 next(3번)와 연결한다.  
    그럼 위에서 다음 노드(2번)라 설명한 중간 노드는 체인이 끊기면서 list에서 사라진다.  
    - defer 구문에서 if문으로 노드의 next가 tail이면 tail이 노드가 되고  
    - if문을 탈출한 후, 노드의 next를 노드의 next의 next와 연결한다.  
    - defer 구문 밖에서 노드의 next의 값을 반환하고 defer 구문을 실행한다.

## Swift collection protocols

---

- Swift standard library는 특정 타입을 정의하는데 도움을 주는 프로토콜의 모음을 가지고 있다.  
    각 프로토콜은 특성과 성능에 대한 보증을 제공한다.  
    프로토콜 모음 중 4개의 Collection 프로토콜이 계층으로 존재하는데, 아래와 같다.  
    1. Sequence: 이 프로토콜은 요소에 대한 순차적인 접근을 제공한다.  
    2. Collection: 추가적인 보장(유한, 비파괴)을 해주는 Sequence 타입이다.  
        유한하며, 반복적인 비파괴적 접근을 제공한다.(?)  
    3. BidirectionalCollection: 양방향 이동을 제공하는 Collection 타입이다.  
    4. RandomAccessCollection: 특정 인덱스의 요소의 접근하는 시간과 다른 인덱스 요소에 접근하는 시간이 동일한 것을 보장하는 BidirectionalCollection 타입이다.  

## Key points of the chapter

---

- Linked list는 직선 형태이며, 단일 방향 접근만 가능하다.  
    한 노드에서 다른 노드로 참조를 이동하고 나면 다시 돌아갈 수 없다.  
- Linked list는 제일 앞에 요소를 추가하는데 O(1)의 시간 복잡도를 갖지만,  
    Array는 O(n)의 시간 복잡도를 가집니다.  
- Copy-on-write는 참조 타입이 아닌, 값 타입을 갖도록 동작합니다.  

---

- 소스코드는 저작권 보호를 위해 삭제하였습니다.  
- Chapter 7는 문제 / 풀이 내용이라 건너뜁니다.
