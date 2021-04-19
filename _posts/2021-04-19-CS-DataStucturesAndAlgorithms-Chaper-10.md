---
layout: post
title: "Data Structures & Algorithms Chapter 10: Trees"
date: 2021-04-19
category: read 
excerpt: ""


---

# [CS-Data Structures & Algorithms]  Chapter 10: Trees

- Tree는 매우 중요한 자료구조이며 소프트웨어 개발의 다양한 측면에서 사용되는데, 간단한 예로
    1. 계층적 관계 표현  
    2. 정렬된 데이터 관리  
    3. 빠른 검색  
  
    Tree는 아주 많은 타입이 있으며, 타입에 따라 모양과 사이즈가 다릅니다.  

## Terminology(용어)

---

### Node

- Linked list와 같이, Tree는 노드로 구성되어 있습니다.  
- 각 노드는 데이터를 가지며, 자신의 자식 노드를 추적합니다.

### Parent and Child

- Tree는 최상단으로 부터 가지를 치듯이(나무와 같이) 아래로 내려옵니다. 위에서 내려오는 것만 가능합니다.  
- 모든 노드는 자신의 상위 노드 하나에 연결되어 있습니다.  
    이 상위 노드를 부모(Parent) 노드라 부르고,  
    그 노드 바로 아래에 있는 노드를 자식(Child) 노드라 부릅니다.  

### Root

- Tree에서 가장 위에 존재하는 노드를 Root(뿌리)라 부르며,  
    한 개의 Tree에서 유일하게 부모를 갖지 않는 노드입니다.

### Leaf

- 자식 노드가 없는 노드를 Leaf(잎)라 부릅니다.

## Implementation(구현)

---

### Node

- 제네릭 클래스로 노드를 정의한다.

- value 프로퍼티를 제네릭 타입으로, children 프로퍼티를 노드 타입의 Array로 정의한다.  
    (Array는 최초 값을 빈 Array([])로 설정한다.)

- 이니셜라이저는 value를 넣으면, value를 가진 노드가 만들어지도록 한다.

- 해당 노드의 children에 append하는 add 메서드를 구현한다.

### Traversal algorithms(순회 알고리즘)

- 직선 형태의 컬렉션(array나 linked list와 같은)은 앞에서 부터 뒤까지 순서대로 진행하면 순회가 가능하지만,  
    Tree 형태의 컬렉션은 해결하려는 문제, 트리의 형태에 따라 다른 전략(알고리즘)을 사용합니다.

- Depth-first traversal
    - 매개변수로 받은 함수에서 자신을 실행한 후, 재귀 프로세스로 children의 각 요소를 호출합니다.

    ```swift
    class Node {
        func depthFirstTraverse(visit: (Node) -> Void) {
    				visit(self)
    				children.forEach {
    					$0.depthFirstTraverse(visit: visit)
    				}
    		}
    }
    ```

- Level-order traversal
    - Level을 기준으로 순회하는 알고리즘입니다.  
        (Level: Root에서 떨어진 정도를 뜻합니다. 예를 들어, Root는 Level 0이고, Root의 자식 노드는 Level 1입니다.)

    - children Array를 forEach를 통해 큐에 넣고, 큐에서 순서대로 빼내서 실행시키면, 레벨을 기준으로 순회합니다.

    ```swift
    func levelOrderTraverse(visit: (Node) -> Void) {
    		visit(self)
    		var queue = Queue<Node>()
    		children.forEach { queue.enqueue($0) }
    		while let node = queue.dequeue() {
    				visit(node)
    				node.children.forEach { queue.enqueue($0) }
    				}
    		}
    }
    ```

- Search
    - 모든 노드를 순회하는 메서드가 있기 때문에, 탐색 알고리즘도 쉽게 구현할 수 있습니다.
    
    - value를 매개변수로 받고 Node를 반환하는 search 메서드를 정의합니다.  
        내부에서는 Depth-first traversal을 통해 Tree를 순회하며 주어진 value와 같은 값을 가진 노드를 찾아서 반환합니다.

    ```swift
    func search(_ value: T) -> Node? {
    		var result: Node?
    		depthFirstTraverse { node in
    				if node.value == value {
    						result = node
    				}
    		}
    		return result
    }
    ```

## Key points of the chapter

---

- Tree와 Linked list는 일부 유사한 점이 있지만, Linked list는 후속 노드 1개만 연결하는 반면에,  
    Tree는 다수의 자식 노드와 연결할 수 있습니다.

- 뿌리 노드(root node)를 제외한 트리의 모든 노드는 정확히 한 개의 부모 노드를 가집니다.

- 뿌리 노드는 부모 노드를 가지지 않습니다.

- 잎(Leaf) 노드는 자식 노드를 가지지 않습니다.

- parent, child, leaf, root와 같은 용어와 익숙해져야 합니다.  
    위 용어들은 다른 트리 구조를 설명할 때 사용되며, 동료 프로그래머들과 대화할 때 자주 사용됩니다.

- Depth-first와 level-order 같은 순회는 기본 트리와 같은 특정 트리에 종속되지 않고,  
    다른 형태의 트리에서도 문제없이 실행됩니다.(트리의 구조에 따라 구현 방식이 변경될 수도 있습니다.)
