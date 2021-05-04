---
layout: post
title: "Data Structures & Algorithms Chapter 36: Graphs"
date: 2021-05-04
category: read 
excerpt: ""


---

# [CS-Data Structures & Algorithms] Chapter 36: Graphs

- 그래프는 객채 간의 관계를 유지하는 자료구조이다. 그리고 이는 선분(edge)에 연결된 꼭짓점(vertex)들로 구성된다.  
  
    ![https://user-images.githubusercontent.com/46529663/116674612-d9b12b00-a9df-11eb-9945-807e03d26742.png](https://user-images.githubusercontent.com/46529663/116674612-d9b12b00-a9df-11eb-9945-807e03d26742.png)

## Weighted graphs

---

- 가중 그래프(Weighted graph)란, 그래프의 모든 선분(edge)에 해당 선분을 사용할 때 드는 비용을 뜻하는 가중치가 부여되는 것을 뜻한다.  
  
- 각각의 선분을 이용할 때 비용이 발생하기 때문에 비용이 적은 선분을 사용할 지, 가장 짧은 선분을 사용할 지 선택해야 한다.  
  
- 예를 들어, 대구에서 부산을 차량으로 이동할 경우 경부 고속도로와 대구-부산 고속도로를 통해갈 수 있는데,  
경부 고속도로는 통행료가 저렴한 대신 거리가 길고, 대구-부산 고속도로는 비싼대신 거리가 짧다.  
(찾아보니, 경부 고속도로가 싼 것은 아니지만 예시에서는 싼 걸로 하자)  

## Directed graphs

---

- 방향 그래프(Directed graph)란, 그래프의 edge에 방향이 설정된 것을 뜻한다.  
특정 방향으로의 이동만 허락하는 edge도 있고, 양방향 이동이 가능한 edge도 있다.

## Undirected graphs

---

- 무방향 그래프(Undirected graphs)란, 그래프 내의 모든 edge가 양방향(bi-directional) 이동이 가능한 것을 뜻한다.  

## Common operations

---

아래부터는 그래프를 위한 프로토콜을 정의한다.  

- enum으로 Edge 타입을 정의한다.(directed or undirected)  
- Graph 프로토콜을 정의하고, 연관 타입과 아래 메서드들을 정의한다.  
    - createVertex: 꼭짓점을 생성한다.  
    - addDirectedEdge: 두 꼭짓점 사이에 방향을 가지는 edge를 추가한다  
    - addUndirectedEdge: 두 꼭짓점 사이에 방향이 없는(양방향 이동이 가능한) edge를 추가한다.  
    - add: enum으로 정의한 EdgeType을 매개변수로 받아서 Directed 또는 Undirected edge를 추가한다.  
    - edges: 특정 꼭짓점(vertex)에서 출발하는 edge들을 반환한다.  
    - weight: 특정 꼭짓점에서 목적지로 이동하는 비용(가중치)를 반환한다.  

## Defining a vertex

---

Vertex 타입(구조체)는 아래와 같이 추가한다.  

- 제네릭 타입을 가지는 Vertex 구조체를 선언한다.  
    - 내부에 Int 타입의 index 프로퍼티와 제네릭 타입의 data 프로퍼티를 선언한다.  
- Dictoinary의 key 타입과 같이 사용하기 위해, Hashable 프로토콜을 채택해야 한다.  
    - 그리고 Hashable 프로토콜은 Equatable 프로토콜을 상속하기 때문에, 두 프로토콜 모두 만족시켜야 한다.  
    - 컴파일러가 두 프로토콜의 적합성을 합성하므로 중괄호 내부는 비울 수 있다.  

## Defining an edge

---

Edge 타입(구조체)는 아래와 같이 추가한다.  

- 제네릭 타입을 가지는 Edge 구조체를 선언한다.  
    - 내부 프로퍼티로 Vertex 타입의 source와 destination 그리고 Double 옵셔널 타입의 weight를 정의한다.  

## Adjacency list

---

그래프는 두가지 모습으로 표현할 수 있는데, 처음으로 구현하는 그래프는 인접 리스트이다.  
인접 리스트의 모든 꼭짓점에 대해 그래프는 해당 꼭짓점과 연결된 선분(edge) 목록을 저장한다.  
다른 곳에서 찾아보면 Linked list 자료구조를 사용하여 그래프의 연결상태를 나타낸다고 한다.  

### Implementation(구현)

- Hashable 프로토콜을 채택하는 제네릭 타입을 가지고, 그래프 프로토콜을 채택하는 AdjacencyList  클래스를 선언한다.  
    - Vertex를 key로, Edge를 value로 가지는 빈 딕셔너리를 선언하고  
    - 빈 이니셜라이저를 정의한다.  

### Creating a Vertex

내부 메서드로 제네릭 타입 data 매개변수를 받아서 Vertex 타입을 반환하는 createVertex 메서드를 선언한다.  

- 내부에서 Vertex 타입(매개변수: adjacencies.count / data: data) 상수 vertex를 선언한다.  
- adjacencies 딕셔너리의 vertex를 키 값으로 한 value를 빈 어레이( [] )로 정의한다.  
- vertex를 반환한다.

### Creating a directed edge

addDirectedEdge 메서드를 아래와 같이 선언한다.  

- Graph 프로토콜에 있는 addDirectedEdge 메서드의 요구사항을 충족한 후  
    - Edge 타입으로 edge 상수를 초기화 한다.  
    - adjaciencies 딕셔너리에서 source 값에 edge를 추가(append)한다.  
    (Source(출발) Vertex에 Direct edge를 추가하는 동작을 한다.)  

### Creating an undirected edge

Undirected edge는 양방향 이동이 가능해야 하기 때문에, addDirectedEdge 메서드를 양쪽에서 사용해서 구현할 수 있다.  
이 구현은 재사용이 가능하므로 Graph 프로토콜에 extension으로 추가한다.  

- Graph 프로토콜의 addUndirectedEdge 메서드를 정의하고  
    - 내부에서 addDirectedEdge를 Source와 Destination을 바꿔서 2회 호출한다.(양쪽에서 연결)  

### Creating an add method

add 메서드는 위 addDirectedEdge와 addUndirectedEdge를 이용하여 쉽게 구현할 수 있다.  

- Graph 프로토콜을 준수하여 add 메서드를 선언한다.  
    - 내부 switch 문을 통해, EdgeType이 .directed이면 addDirectedEdge를,   
    .undirected라면 addUndirectedEdge를 호출한다.

### Creating an edges method

특정 Vertex에서 나오는 edge의 배열을 반환하는 메서드는 아래와 같이 구현한다.  

- Vertex 타입의 source를 매개변수로 받아서 Edge 타입의 배열을 반환한다.  
    - 내부에서는 adjacencies 딕셔너리에 source를 키 값으로 주면, 해당 소스의 값(Edges)을 반환한다.  

### Creating a weight method

특정 Vertex에서 다른 Vertex와 연결된 Edge의 weight를 반환하는 메서드는 아래와 같이 구현한다.  

- Vertex 타입의 source와 destination을 매개변수로 받고 옵셔널 Double 타입을 반환한다.  
    - 내부에서는 edges 메서드를 호출하여, source Vertex의 edge 배열을 받아오고,  
    first 메서드로 매개변수로 주어진 destination과 같은 edge를 찾는다.  
    - 찾으면 .weight를 통해 Double 타입의 가중치를 반환한다.  

### Visualizing the adjacency list

만든 인접 리스트 그래프를 출력하기 위해 CustomStringConvertible 프로토콜을 채택합니다.  

- 익스텐션을 통해 AdjacencyList에 CustomStringConvertible 프로토콜을 추가하고  
    - 비어있는 String 타입의 result 변수를 추가한다.  
    - for 문으로 adjacencies 내부 키와 값을 받아온다.  
        - 빈 String 타입 변수 edgeString을 추가한다.  
        - for 문으로 edges의 각 요소를 (index, edge) 형태로 순회한다.  
            - edgeString에 edge의 destination을 추가한다.  
        - result에 현재 vertex를 추가하고, edgeString을 추가한다.(destination들의 모음)  
    - result를 반환한다.  

## Building a network

---

비행기가 이동하는 네트워크를 코드로 만들어보면 아래와 같다.  

- AdjacencyList<String> 타입의 graph를 초기화한다.  
- 각 도시에 해당하는 상수를 graph의 createVertex 메서드를 사용해서 초기화한다.  
- graph의 add 메서드를 사용해 edge를 생성한다.(엣지 타입, 출발지, 도착지, 비용을 매개변수로 준다.)  
  
```swift
// Output은 아래와 같다.
Vertex<String>(index: 6, data: "Austin Texas") ---> [ Vertex<String>(index: 3, data: "Detroit"), Vertex<String>(index: 5, data: "Washington DC"), Vertex<String>(index: 4, data: "San Francisco") ]
Vertex<String>(index: 2, data: "Hong Kong") ---> [ Vertex<String>(index: 0, data: "Singapore"), Vertex<String>(index: 1, data: "Tokyo"), Vertex<String>(index: 4, data: "San Francisco") ]
Vertex<String>(index: 1, data: "Tokyo") ---> [ Vertex<String>(index: 0, data: "Singapore"), Vertex<String>(index: 2, data: "Hong Kong"), Vertex<String>(index: 3, data: "Detroit"), Vertex<String>(index: 5, data: "Washington DC") ]
Vertex<String>(index: 0, data: "Singapore") ---> [ Vertex<String>(index: 2, data: "Hong Kong"), Vertex<String>(index: 1, data: "Tokyo") ]
Vertex<String>(index: 3, data: "Detroit") ---> [ Vertex<String>(index: 1, data: "Tokyo"), Vertex<String>(index: 6, data: "Austin Texas") ]
Vertex<String>(index: 5, data: "Washington DC") ---> [ Vertex<String>(index: 1, data: "Tokyo"), Vertex<String>(index: 6, data: "Austin Texas"), Vertex<String>(index: 4, data: "San Francisco"), Vertex<String>(index: 7, data: "Seattle") ]
Vertex<String>(index: 7, data: "Seattle") ---> [ Vertex<String>(index: 5, data: "Washington DC"), Vertex<String>(index: 4, data: "San Francisco") ]
Vertex<String>(index: 4, data: "San Francisco") ---> [ Vertex<String>(index: 2, data: "Hong Kong"), Vertex<String>(index: 5, data: "Washington DC"), Vertex<String>(index: 7, data: "Seattle"), Vertex<String>(index: 6, data: "Austin Texas") ]
```

## Adjacency matrix

---

그래프를 표현하는 방법 중, 두 번째 방법으로 인접 행열으로 구현한다.  

행열은 2차원 배열을 사용해 행과 열을 구분하여 나타낸다.  

### Example

0 = Singapore  

1 = Hong Kong  

2 = Tokyo  

3 = Washington DC  

4 = San Francisco  

![https://user-images.githubusercontent.com/46529663/116981664-cf09d500-ad02-11eb-8a20-bb36f5c427f5.png](https://user-images.githubusercontent.com/46529663/116981664-cf09d500-ad02-11eb-8a20-bb36f5c427f5.png)

위 예시와 같이 행열을 표현할 수 있으며 각 비용은 아래와 같이 읽을 수 있다.  

- [0] [1]은 Singapore - Hong Kong Edge를 뜻하며 비용은 300이다.  
- [2] [3]은 Tokyo - Washington DC Edge를 뜻하며 비용은 300이다.

### Implementation

- 제네릭 타입을 가지고 그래프 프로토콜을 채택하는 AdjacencyList  클래스를 선언한다.  
    - 내부에 Vertex 타입의 배열 변수 vertices와 옵셔널 Double 타입의 이차원 배열을 정의하고,  
    빈 이니셜라이저를 생성한다.

### Creating a Vertex

- 그래프 프로토콜을 준수하는 createVertex 메서드를 생성한다.  
    - 내부에서 Vertex 타입의 vertex 변수를 선언하고, vertices에 추가(append)한다.  
    - 새로 생성하는 Vertex는 무게를 가지는 Edge가 없기 때문에,  
    for 문으로 0..<weights.count 범위에 nil을 추가(append)한다.  
    (모든 열에 자신의 칸을 추가한다.)  
    - 상수 row를 옵셔널 Double 타입 어레이를 repeating을 통해 선언한다.  
    (매개변수 count: vertices.count)  
    - row를 weights에 추가하고(자신을 weights 배열에 열으로 추가한다.), vertex를 반환한다.  

### Creating edges

- 그래프 프로토콜을 준수하는 addDirectedEdge 메서드를 생성한다.  
    - 내부에서 weights의 source 행의 destination 열의 값을 주어진 weight로 설정한다.  
  
addUndirectedEdge 메서드는 익스텐션을 통해 구현이 되었으므로, 다시 작성할 필요가 없다.

### Creating an edges method

- 그래프 프로토콜을 준수하는 edges 메서드를 생성한다.  
    - Edge 타입의 배열인 edges 변수를 빈 배열로 초기화한다.  
    - for 문으로 0..<weights.count 범위를 column에 넣는다.  
    - if 문 옵셔널 바인딩을 통해 해당 행의 weight가 nil이 아니면 weight 상수에 값을 넣는다.  
    - 바인딩이 성공하면 edges에 해당 Edge를 넣고, 아니라면 다음 열(for문 내에서)로 넘어간다.  
    - for 문을 탈출하면 edges를 반환한다.

### Creating a weight method

- 그래프 프로토콜을 준수하는 weight 메서드를 생성한다.  
    - 내부에서 weights 배열 내의 source 행의 destination 열의 값을 반환한다.(특정 edge)  

### Visualize an adjacency matrix

- 익스텐션을 통해 CustomStringConvertible 프로토콜을 채택한다.  
    - 익스텐션 내에 description 변수를 선언한다.  
        - 내부에 verticesDescription 상수를 vertices를 매핑해서 joined(separator:) 메서드로 줄을 나눈다.  
        - String 타입의 배열인 grid 변수를 빈 배열로 초기화하고, for 문으로 0..<weights.count(행) 범위를 i 에 주면서 반복한다.  
            - String 타입의 row 변수를 빈 문자열로 초기화한다.  
            - for 문으로 0..<weights.count(열) 범위를 j에 주고 반복한다.  
                - weights[i][j]의 값이 유효하면 value(+ /t)를 row 문자열에 추가하고,  
                유효하지 않으면(nil) ø(+ /t/t)를 추가한다.
            - row를 grid에 추가(append)한다.  
        - edgesDescription 상수를 grid에 joined(seperator:) 메서드를 사용하여 초기화한다.  
        - verticesDescription과 edgesDescription 사이에 2줄을 띄운 String을 반환한다.

    ### Building a network

    기존에 구현했던 네트워크에서 graph의 타입을 AdjacencyList에서 AdjacencyMatrix로 바꾼다.  

    ```swift
    // Output은 아래와 같다.
    Vertex<String>(index: 0, data: "Singapore")
    Vertex<String>(index: 1, data: "Tokyo")
    Vertex<String>(index: 2, data: "Hong Kong")
    Vertex<String>(index: 3, data: "Detroit")
    Vertex<String>(index: 4, data: "San Francisco")
    Vertex<String>(index: 5, data: "Washington DC")
    Vertex<String>(index: 6, data: "Austin Texas")
    Vertex<String>(index: 7, data: "Seattle")

    ø		500.0	300.0	ø		ø		ø		ø		ø		
    500.0	ø		250.0	450.0	ø		300.0	ø		ø		
    300.0	250.0	ø		ø		600.0	ø		ø		ø		
    ø		450.0	ø		ø		ø		ø		50.0	ø		
    ø		ø		600.0	ø		ø		337.0	297.0	218.0	
    ø		300.0	ø		ø		337.0	ø		292.0	277.0	
    ø		ø		ø		50.0	297.0	292.0	ø		ø		
    ø		ø		ø		ø		218.0	277.0	ø		ø
    ```

## Graph analysis

아래 표는 인접 리스트와 인접 행렬의 각 작업에 대한 비용이다.  

![https://user-images.githubusercontent.com/46529663/117011808-3175cc00-ad29-11eb-9d6d-aa31a9c1fa71.png](https://user-images.githubusercontent.com/46529663/117011808-3175cc00-ad29-11eb-9d6d-aa31a9c1fa71.png)

위 표에서 V는 꼭짓점들(vertices)을, E는 선분들(Edges)를 나타낸다.  
O(V2)는 제곱을 표현한 것이다.  

1. Storage Space  
    - List: 단순히 Vertices의 개수에 Edge의 개수만큼 넣은 비용을 가진다.  
    - Matrix: Vertices의 개수만큼의 행과 열을 가지므로 V 제곱의 비용을 가진다.  
2. Add Vertex  
    - List: 한 쌍의 Key-Value를 딕셔너리 내에 생성한다.  
    - Matrix: Vertex를 추가하면, 새 열을 모든 행에 추가하고 새로운 행을 하나 추가해야한다.  
3. Add Edge  
    - 두 자료구조 모두 상수 시간을 가진다.  
    - List: 배열에 Edge를 추가한다.  
    - Matrix: 2차원 배열에 Edge를 추가한다.  
4. Finding Edges and Weight  
    - List: 인접 리스트에서 특정 edge를 찾기 위해서는 해당 edge를 포함한 Vertex를 찾고 목적지가 매칭하는 edge를 루프를 통해 찾아야한다.  
    - Matrix: 이차원 배열에서 값을 받아오는데에 상수 시간이 걸린다.  

## Key points of the chapter

- Vertices와 Edges를 통해 실제 세계에서의 관계를 표현할 수 있다.  
- Vertices를 객체로, Edges를 객체 간의 관계라고 생각하면 된다.  
- 가중 그래프는 모든 선분에 가중치를 부여한다.  
- 방향 그래프들은 한 방향으로 이동하는 선분들을 가진다.  
- 무방향 그래프는 양방향을 가르키는 선분이 있다.  
- 인접 리스트는 모든 꼭짓점에 꼭짓점에서 나가는 선분을 저장한다.  
- 인접 행렬은 정사각 행렬을 이용하여 그래프를 표현한다.  
- 인접 리스트는 간단한 그래프 즉, 최소한의 선분을 가질 때 사용하면 좋다.  
- 인접 행렬은 밀도가 높은 그래프 즉, 선분이 많은 그래프를 구현할 때 사용하면 좋다.  
