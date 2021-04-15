---
layout: post
title: "Data Structures & Algorithms Chapter 3: Swift Standard Library"
date: 2021-04-12
category: read 
excerpt: ""


---

# [CS-Data Structures & Algorithms] Chapter 3: Swift Standard Library

## Swift Standard Library란?

---

- 스위프트 언어의 주요 요소를 포함한 프레임워크
- 다양한 툴과 타입들을 자료구조를 구현하는데 사용할 것이고,  
    이미 Swift Standard Libarary에서 제공하는 자료구조도 있다.

- 이 챕터에서 Swift Standard Library가 제공하는 세가지 주요 자료구조를 살펴보자.  
    (Array, Dictionary, Set)

## Array

---

- Array는 요소들을 순서대로 저장하기 위한 generic collection이며,  
    모든 종류의 Swift 프로그램에서 사용된다.  
- 스위프트는 Array를 프로토콜을 통해 정의했고,  
    이 프로토콜들은 Arrya에 더 많은 기능을 계층화해서 제공한다.  
    예를 들어, Array는 Sequence 프로토콜을 채택하는데,   
    이는 Array가 적어도 한번은 반복할 수 있다는 말이다.  
    그리고 Collection 프로토콜도 채택하는데,   
    이는 여러번 횡단(?)할 수 있고 subscript operator(array[i])을 사용하여 접근할 수 있다.  
    마지막 예로 RandomAccessCollection 프로토콜을 채택하는데,   
    이는 효율성을 보장한다(?).  
- 스위프트의 Array는 generic collection으로 알려져있는데,  
    어떤 타입이든 사용이 가능하기 때문이다.  
    (사실은 Swift Standard Library 코드의 대부분은 generic type으로 구현되어 있다.)  
- Order(Array는 순서대로 구현된다.)  
    Array 내의 모든 요소는 0부터 시작하는 정수 인덱스를 가진다.  
    순서는 자료구조에 의해 정의되며, 당연하게 여길 수는 없다.  
    예를 들어, Dictionary는 순서 개념이 약하다(없다?).  
- Random-access  
    Random-access란, 일정 시간 내에 요소 검색을 처리하는 자료구조이다.  
    이 또한, 당연시 여길 수 없고 Linked list,Tree와 같이 Random-access가 정의되지 않은  
    다른 자료 구조는 요소 검색에 일정한 시간을 가지지 않는다.  
    (검색하는 요소에 따라 시간이 다름)

- Array performance  
    Random-access외에도 개발자가 신경써야 할 성능적인 부분이 있는데,  
    특히 데이터의 양을 늘려야 할 때, 데이터 구조가 얼마나 잘 작동하는 지는 두가지 요인에 따라 다르다.
    1. Insertion location(삽입 위치)  
        우선 가장 좋은 삽입 위치는 새로운 요소를 Array의 마지막에 넣는 것이다.  
        쉽게 생각해보면, 음식 사기위해 줄을 서 있다고 생각해보자.  
        새로운 사람이 왔을 때, 가장 좋은(일반적인) 케이스는 맨 뒤에 서는 것이다.  
        안좋은 케이스는 그 사람이 중간에 끼어드는 것이며(그 뒤에 있는 사람들은 모두 한칸씩 뒤로 밀린다.)  
        제일 안좋은 케이스는 맨 앞에 끼어드는 것이다.(줄에 있는 모든 사람을 뒤로 한칸씩 보낸다.)  
        Array도 위 예시와 같이 맨 앞에 끼어들면 모든 요소가 뒤로 한칸씩 밀린다.  
        만약 내 프로그램에서 요소를 배열의 맨 앞에 삽입하는 작업을 많이 사용한다면, 다른 자료구조를 사용하는 것이 더 효율적일 것이다.  
    2. Capacity(용량)  
        메모리에서 Swift의 Array는 정해진 양의 공간이 할당됨  
        그런데, 이미 가득찬 Array에 새로운 요소를 추가하려하면 새로운 공간을 확보하기 위해 Array를 재구성 해야함  
        더 큰 메모리 공간에 복사해서 수행하기 때문에 비용이 발생하고, 이는 성능을 저하시키고, 작업을 느리게 한다.  
        
## Dictionary

---

- Dictionary는 Key-value 쌍을 가지는 다른 generic collection이다.  
- Dictionary는 순서를 보장하지 않으며, 특정 위치에 요소를 삽입할 수 없다.  
- Key type은 Hashable 프로토콜을 준수해야 하는데, 다행히 대부분의 스위프트 기본 타입들은 Hashable을 준수한다.  
- Collection 프로토콜이 제공되는 동안은 key-value를 여러 번 탐색할 수 있다.  
- Array와 달리 요소 이동(elements shifting)을 신경 쓸 필요가 없다.(Dictionary는 요소 삽입에 언제나 일정한 시간만 소요한다.)  
- 탐색 작업 또한 일정 시간만을 소요하며, 이는 배열이 탐색하는 것보다 훨씬 빠르다.

## Set

---

- Set은 유일한 값만 가지는 컨테이너이다.(이미 가지고 있는 값은 넣을 수 없다.)  
- 아마 Set은 Dictionary, Array와 같은 자료구조 보다는 덜 사용하겠지만,   
    충분히 알고있으면 좋을만한, 중요한 자료구조이다.  
- Dictionary와 같이 순서에 대한 개념이 없으므로, 사용에 유의하자!  

## Key points of the chapter

---

- 모든 자료구조는 장단점이 있다.  
    장단점을 아는 것이 퍼포먼스 소프트웨어를 작성하는 키이다.
- Insert(at:)과 같은 함수는 때로 성능을 저하시킬 수 있다.  
    자주 Insert(at:)을 사용해 Array의 시작과 가까운 곳에 값을 삽입해야한다면,  
    다른 자료구조 사용을 검토해봐야 한다.  
- Dictionary는 빠른 검색을 위해 순서를 유지하는 기능을 포기했다.  
- Set은 값의 고유성을 보장하고, 속도에 최적화 되어있지만,  
    순서를 유지하는 기능을 포기했다.
