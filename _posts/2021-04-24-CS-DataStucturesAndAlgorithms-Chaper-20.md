---
layout: post
title: "Data Structures & Algorithms Chapter 20: Binary Search"
date: 2021-04-24
category: read 
excerpt: ""


---

# Chapter 20: Binary Search

- 이진 탐색은 가장 효과적인 탐색 알고리즘 중 하나로, O(log n)의 시간 복잡도를 가진다.
    - 이진 탐색이 사용 되기 위해서는 두 가지가 필요한데,  
        1. 컬렉션이 RandomAccessCollection이어야 한다.(인덱스 사용에 상수 시간을 가진다.)  
        2. 컬렉션이 정렬되어 있어야 한다.

## Example

---

- 이진 탐색을 이해하기 위해 좋은 방법은 선형 탐색(linear search)과 비교하는 것인데,  
    Swift의 Array는 firstIndex(of:) 메서드를 선형탐색으로 구현해놓았다.  
    
    만약 [1, 2, 3, 4, 5, 6].firstIndex(of:6)를 하면, 최악의 경우로 6회 만에 찾는다.  
    그에 대하여, 이진 탐색을 사용한다면 처음에 4로 접근했다가 6으로 가게 되어 2회 만에 찾는다.
  
    어떻게 작동되는 지 알아보면  
    - Step 1: 중간 인덱스를 찾는다.(3)  
    - Step 2: 중간 인덱스에 있는 요소가 찾는 값과 같은 지 확인한다.  
    - Step 3: 찾는 값이 중간 인덱스 요소보다 작으면 왼쪽, 크면 오른쪽 배열에 재귀적으로 이진탐색을 호출한다.  
    - 위 Step을 반복하며 Array를 반씩 자르면서 해당 값을 찾아간다.  

## Implementation

---

- 이진 탐색은 RandomAccessCollection 프로토콜을 준수하는 컬렉션에만 사용할 수 있기 때문에, extension을 통해 binarySearch 메서드를 추가하는 것으로 구현한다.  
  
    매개변수로는 제네릭(Element) 타입의 찾는 값(value),   
    Range<Index>? 타입의 범위(range)를 받는다.(range의 기본 값은 nil이다.)  
    
    1. range가 nil이면, startIndex..<endIndex를 range에 넣고, 아니면 그대로 range 값을 가진다.  
    2. guard를 통해 range.lowerBound가 range.upperBound보다 작다는 걸 확인한다.  
    3. distance(from:to:) 메서드를 이용해서, 준 범위의 거리를 size 상수에 저장하고  
        size / 2를 오프셋으로 가지는 인덱스를 middle 상수에 넣는다.  
    4. self[middle](중간 인덱스에 있는 요소)가 찾는 값(value)와 같으면 middle을 반환한다.  
    5. 아니라면 self[middle]이 value 보다 큰지, 작은지 확인하고  
        크면 range.lowerBound..<middle 범위를 이진 탐색(재귀)하고  
        작으면 index(after: middle)..<range.upperBound 범위를 이진 탐색(재귀)한다.

## Key points of the chapter

---

- 이진 탐색은 정렬된 컬렉션에서만 사용할 수 있다.  
- 때로는 이진 탐색을 사용하기 위해 컬렉션을 정렬하는 것이 유용할 수도 있다.  
- 큰 데이터를 가진 컬렉션을 반복적으로 탐색할 때는, 선형 탐색보다 이진 탐색으로 처리하는 것이 훨씬 효과적이다.
