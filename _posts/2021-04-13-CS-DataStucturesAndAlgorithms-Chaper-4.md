---
layout: post
title: "Data Structures & Algorithms Chapter 4: Stack Data Structure"
date: 2021-04-13
category: read 
excerpt: ""


---

# [CS-Data Structures & Algorithms] Chapter 4: Stack Data Structure

- 스택은 어디에나 있다. 아래 예시는 일반적으로 스택을 사용하는 요소들이다.
    - pancakes
    - books
    - paper
    - cash
- Stack 자료구조는 물리적으로 포개서 사용하는 객체들과 개념적으로 같다.  
    Stack에 요소를 추가할 때, 가장 위에 추가하고 / 요소를 제거할 때도 가장 위에서부터 제거한다.

## Stack Operation
---
- 스택은 유용하고 아주 심플하다.  
- 스택의 목표는 데이터 접근 방식을 적용하는 것이다.  
   LinkedList와 같은 자료구조을 사용하다가 스택을 사용하면 비교적 간단하게 느낄 수 있다.  
- 스택의 두가지 주요 작업  
    1. push: 요소를 스택 가장 위에 추가  
    2. pop: 스택 가장 위의 요소를 제거  
- 스택에는 위 두가지 작업 밖에 존재하지 않으며, 이는 스택이 일방적으로만 요소를 추가하고 제거함을 뜻한다.  
    (그래서 Stack은 LIFO(Last - in  - first - out) 자료구조로 알려져있다.

- 스택은 프로그램을 구현할 때, 눈에 띄게 많이 사용되는데, 대표적으로:  
    - iOS의 navigation stack은 View Controller를 뷰에게 push / pop 한다.(스택 구조로 쌓임)  
    - 메모리 할당할 때에도 아키텍쳐 수준에서는 stack이 사용되며, 지역 변수의 메모리도 스택으로 관리된다.  
    - Search and conquer 알고리즘에서는 역추적을 용이하게 하기 위해 스택을 사용한다.

## Implementation(구현)  
---
- 제네릭 구조체로 Stack을 생성하고, 내부에 Array 타입의 storage를 프로퍼티로 정의한다.  
- push는 storage에 append()로 / pop은 storage에 removeLast()로 구현한다.

## Key points of the chapter
---
- Stack은 LIFO(Last - in - first - out) 자료구조이다.  
- 간단하게 보일지라도 Stack은 많은 문제에 대해 해결책인 자료구조로 사용된다.  
- Stack의 중요한 두가지 작업은 요소를 추가하는 push 메서드와 제거하는 pop 메서드이다.

---

- 소스코드는 저작권 보호를 위해 삭제하였습니다.  
- Chapter 5는 문제 / 풀이 내용이라 건너뜁니다.
