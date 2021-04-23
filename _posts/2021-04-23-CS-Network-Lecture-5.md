---
layout: post
title: "Network Lecture 5: Transport Layer (1)"
date: 2021-04-23
category: read 
excerpt: ""


---

# Lecture 5: Transport Layer (1)

## Performance of rdf3.0

---

- Lecture 4에서 살펴본 rdt3.0은 동작은 하지만, 성능적으로 심히 부족하다.  
    예를 들어서, 1 Gbps link, 15 ms e-e prop delay, 1KB packet을 가질 때  
    
    Transmit = packet length / bps = 8microsec이 된다.  
    즉, 발송하는데에 걸리는 시간이 8 microsec이다.  

    이렇게 되면 센더가 사용 가능 시간의 0.027%만 사용한다.  
    그리고 1 Gbps의 Link를 가지고 33KB/sec의 데이터만 보내게 된다.  
    (이를 stop and wait이라 부르고 아래 왼쪽 사진과 같다. / TCP에서는 아래 오른쪽 사진과 같이 작동한다.)  
  
    그리고 아래 오른쪽 사진을 pipelined protocol이라 하며, 해당 프로토콜은 두가지 형태로 나뉜다.  
    ![https://user-images.githubusercontent.com/46529663/115864158-78360d00-a471-11eb-8872-8ba49fe18d74.png](https://user-images.githubusercontent.com/46529663/115864158-78360d00-a471-11eb-8872-8ba49fe18d74.png)

## Go-Back-N

---

위에서 말한 pipelined 프로토콜 중 첫 번째 프로토콜이다.

- pipelined 프로토콜의 성격으로 한번에 많은 패킷을 보내게 되는데, 한 번에 얼마만큼 보낼 지 기준이 필요하다.  
    그리고 이 기준을 window라고 부른다.  
    window size만큼은 피드백 받지 않고 보낼 수 있다.  
  
    Go-Back-N의 작동방식은 리시버가 문제 없이 받은 패킷까지의 번호(sequence #)을 보낸다.  
    (ACK11을 받으면 11번 패킷까지는 잘 받았다는 말)  
  
    window size만큼 패킷을 보낼 때, 각각의 패킷에는 타이머가 있는데  
    어느 패킷의 타이머가 끝나면 window안에 해당 패킷보다 뒤에 있는 패킷을 모두 데리고 재전송한다.  

- 리시버는 단순히 순서대로 패킷을 기다린다.  
    아래 예시에서, 1번 패킷이 도착한 후 ACK1을 보내면 이제 2번 패킷을 기다리는데  
    3번, 4번, 5번 등 다른 패킷이 올 경우, 다 버리고 패킷 1의 Sequence #을 가진 ACK1를 보낸다.  
    (패킷 0번, 1번은 문제 없이 처리되었으니, window에서 빠지고 4번 5번이 새로 들어옴)  
    ![https://user-images.githubusercontent.com/46529663/115865969-feebe980-a473-11eb-9e5e-25c945410be3.png](https://user-images.githubusercontent.com/46529663/115865969-feebe980-a473-11eb-9e5e-25c945410be3.png)
  
- 데이터가 유실되면 n(window size)개 만큼 돌아온다고 해서 Go-Back-N이라 부른다.

## Selective Repeat

---

pipelined 프로토콜 중 두 번째 프로토콜이다.  
- Go-Back-N과 다른 점은 특정 패킷을 골라서 돌아간다는 점이다.  
  
- Go-Back-N의 ACK는 해당 ACK의 Sequence #까지 문제 없다는 말이지만,  
    Selective Repeat의 ACK는 해당 ACK의 Sequence #을 가진 패킷만 문제가 없다는 말이다.  
  
- 위 프로토콜에서는 순서에 맞지 않는 패킷이 들어오더라도 저장해야한다.  
    그리고, 각 패킷에 맞는 ACK를 보내준다.  
    ![https://user-images.githubusercontent.com/46529663/115867748-6c007e80-a476-11eb-9f2a-3f4b7e13682e.png](https://user-images.githubusercontent.com/46529663/115867748-6c007e80-a476-11eb-9f2a-3f4b7e13682e.png)
  
- 문제점  
    - seq #: 0, 1, 2, 3 / window size = 3 일 때, 0, 1, 2 패킷을 보낸 후 ACK가 모두 유실되면  
        센더는 0, 1, 2를 다시보내고, 리시버는 3, 0, 1을 기다리기 때문에 3의 다음인 0을 받게 된다.  
        (리시버는 처음 보낸 0과 다시 받은 0의 차이를 모른다.)  
  
    - 해결책: 리시버가 혼동하지 않을 정도의 seq # 범위를 가져야 하는데,  
        최적의 범위는 window size * 2 정도(?)
