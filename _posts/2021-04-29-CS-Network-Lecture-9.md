---
layout: post
title: "Network Lecture 9: Network Layer (1)"
date: 2021-04-29
category: read 
excerpt: ""


---

# [CS-Network] Lecture 9: Network Layer (1)

## Core of Network Layer

---

- Internet Protocol(IP)  
- Routing algorithms

## Forwarding

---

- 라우터는 패킷의 목적지와 어느 경로로 보내야 하는 지 알아야 하는데,  
라우터는 라우터가 가진 테이블(Forwarding table)을 보고 패킷을 전달한다.  
이 작업을 포워딩(Forwarding)이라 부른다.

- 위 포워딩 테이블은 사람이 만들지 않는다.   
    모든 라우터가 몇 개인지 알 수도 없을 뿐더러, 각각에 테이블을 맞게 넣어주면 기하급수적인 시간이 걸린다.  
    그래서 라우팅 알고리즘을 통해 포워딩 테이블을 자동으로 만들어준다.

- 포워딩 테이블에 모든 라우터를 올려놓을 수는 없다.(비효율 및 모든 라우터를 파악하는 것도 거의 불가능에 가까울 듯 하다.)  
그래서 주소를 올려놓고, 몇 번부터 몇 번까지는 n번 라우터... 이렇게 해서 주소별로 묶어서 포워딩한다.  
  
    ![https://user-images.githubusercontent.com/46529663/116562384-11679680-a93e-11eb-8c2d-1a21d73d79ea.png](https://user-images.githubusercontent.com/46529663/116562384-11679680-a93e-11eb-8c2d-1a21d73d79ea.png)
