---
layout: post
title: "Network Lecture 8: Transport Layer (4)"
date: 2021-04-27
category: read 
excerpt: ""


---

# [CS-Network] Lecture 8: Transport Layer (4)

## TCP Congestion Control

---

### 3 main phases  
- Slow Start: 작게 시작해서, 보내는 양을 2배씩 늘리는 방식(0부터 시작해서 빠르게 늘려간다.)  
  
- Additive increase: threshold 값 부터는 보내는 양을 조금씩만 늘린다.(linear 하게)  
  
- Multiplicative decrease: 패킷 유실을 탐지하면, 보내는 양을 절반으로 줄인다.  
    (네트워크는 공유 자원이고 뺄 때 차근차근 빼면 해결이 안되기 때문에, 확 (절반으로)줄인다.)

### Additive increase, multiplicative decrease  
- MSS: Maximum segment size   
  
- Approach(접근법): 패킷 로스가 생길 때 까지, 사용 가능한 bandwitdh를 증명하기 위해 window size를 늘린다.  
    - additive increase: increase Congestion window size by 1 MSS every RTT until loss detected  
    - multiplicative decrease: cut Congestion window size in half after loss  
    
- 위 접근법을 사용하면, 아래와 같이 window size가 변화하고 이를, saw tooth behavior이라 부른다.  
    ![https://user-images.githubusercontent.com/46529663/116204392-b1bf8e80-a777-11eb-87a6-1e0f056bd9eb.png](https://user-images.githubusercontent.com/46529663/116204392-b1bf8e80-a777-11eb-87a6-1e0f056bd9eb.png)

### Details  
- rate = (CongWin / RTT) Bps(Bytes per sec)  

- TCP는 독립적인 PC에서 작동하지만, 네트워크를 통해 서로 영향을 준다.  

- 센더가 congestion을 추측하는 방법
    1. loss event = timeout 또는 3번의 중복된 ACK  
    2. TCP 센더는 데이터 유실 후에 CongWin을 줄인다.(Transmission rate)

### Slow Start  
- 이름은 Slow Start지만, 실제로는 기하급수적으로(exponential) 늘어난다.  

- 처음 연결이 시작할 때, CongWin = 1MSS  
    - Eamxple: MSS = 500 bytes / RTT = 200 msec → Initail rate = 20kbps  
    
- RTT마다 CongWin을 두 배로 증가시킨다.  

- Summary: 최초 rate는 느리지만, 기하급수적으로 증가시킨다.

### Refinement

- Threshold는 가변성을 가지는데 유실이 발생할 경우,  
    이전 CongWin 크기의 절반을 Threshold 값으로 가진다.(최초 Threshold 값은 모른다.)

- TCP는 첫 번째 버전인 Tahoe와 개선 버전인 Reno가 존재하는데,  
    Reno에서 개선된 점은 유실을 감지하는 방법에 따라 CongWin 크기를 다르게 시작한다.  
    - Reno는 유실이 발생하는 경우에 따라 CongWin을 어떻게 다시 시작할 지 정하는데,  
        1. Timeout: 네트워크 전반에 문제가 있다고 추정하고, 처음(slow start)부터 시작  
        2. 3 duplicate acks: 해당 패킷만 문제가 있다고 추정하고, 중간(addtive increase)부터 시작  
    ![https://user-images.githubusercontent.com/46529663/116209304-bc305700-a77c-11eb-837f-5e9697967d43.png](https://user-images.githubusercontent.com/46529663/116209304-bc305700-a77c-11eb-837f-5e9697967d43.png)

## TCP Fairness

---

- Fairness goal: N개의 TCP 세션이 R Link를 공유할 때, 각 세션이 R/N의 평균 레이트를 가지는 것  
- Why is TCP fair?  
    - 두 개의 세션이 있다고 가정할 때,  
        1. Additive increase를 통해 기울기가 1씩 증가한다.  
        2. Multiplicative decrease를 통해 처리량을 비례적으로 감소시킨다.  
        
        결국 비례하게 감소하고, 증가는 같이 하기 때문에 Fair한 값에 점점 가까워진다.  
        (TCP Connection 끼리의 공평함이기 때문에, 소켓이 여러개인 프로세스는 더 많은 네트워크를 사용하게 된다.)  
        ![https://user-images.githubusercontent.com/46529663/116213568-de2bd880-a780-11eb-9fec-c09dfe20813a.png](https://user-images.githubusercontent.com/46529663/116213568-de2bd880-a780-11eb-9fec-c09dfe20813a.png)

## Summary

---

![https://user-images.githubusercontent.com/46529663/116214003-3f53ac00-a781-11eb-87b5-6ece9cb0a460.png](https://user-images.githubusercontent.com/46529663/116214003-3f53ac00-a781-11eb-87b5-6ece9cb0a460.png)
