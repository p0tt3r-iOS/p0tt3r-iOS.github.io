---
layout: post
title: "Network Lecture 7: Transport Layer (3)"
date: 2021-04-26
category: read 
excerpt: ""


---

# [CS-Network] Lecture 7: Transport Layer (3)

## TCP Flow Control

---

- Receiver - drive

- TCP Segment에 있는 receive window에 받을 수 있는 사이즈를 넣어서 보낸다.  
    TCP는 양방향으로 Segment를 보내기 때문에, 보낼 때 넣어서 보내면 양쪽 센더 버퍼가 알 수 있다.

- 특별한 케이스로 리시브 버퍼에 공간이 없을 경우엔 받을 수 있는 사이즈를 0을 보낸다.  
    그러면 상대 편 센더 버퍼는 데이터가 빈 세그먼트를 주기적으로 보낸다.  
    (언제 공간이 생기는 지 알기 위해)

- Flow Control은 아주 중요한 기능이지만, 리시버가 RcvWindow를 보내주기 때문에,  
    간단하게 구현된다.

## TCP Connection Management

---

- TCP의 준비작업: 각 소켓에 센더/리시버 버퍼를 만들고  
    센더에게는 자신의 seq #, 리시버에게는 보내는 소켓의 seq #를 준다.

### Three way handshake

- Step 1: 클라이언트가 Segment Header의 SYN bit = 1으로 설정한 Segment를 서버에게 보낸다.  
    (Seq #를 알려주는 작업 / TCP Connection을 하고 싶다는 의사표현)  
- Step 2: 서버가 클라이언트에게 SYN에 대한 ACK(SYN bit = 1)를 해준다.  
- Step 3: SYNACK에 대한 ACK를 보내준다.(Step 3부터 데이터를 포함할 수 있다.)  
    ![https://user-images.githubusercontent.com/46529663/116079889-589e1f00-a6d3-11eb-8d7e-4d346a7a7962.png](https://user-images.githubusercontent.com/46529663/116079889-589e1f00-a6d3-11eb-8d7e-4d346a7a7962.png)  
  
- 세번 왔다갔다 하는 이유는?  
    서로 데이터(예를 들어 위 예시의 Seq #)를 주고 받을 경우, 두쪽 모두 잘 받았다는 메세지를 받아야 에러가 없는 지 알 수 있다.  
    두번 왔다갔다하면 처음 보낸 주체(위의 클라이언트)는 ACK를 문제 없이 받지만, 두 번째 보내는 주체(서버)는 ACK를 받을 수 없다.

### Closing TCP Connection

- Closing a connection  
    - Client closes socket: colientSocket.close()  
- Step 1: 클라이언트가 보낼 데이터를 다 보낸 경우, FIN bit = 1로 Segment를 보낸다.  
- Step 2: 서버가 클라이언트가 보낸 FIN 세그먼트를 받고 ACK를 보낸 후, 자신이 보낼 데이터를 다 보내면 FIN 세그먼트를 클라이언트에게 보낸다.  
- Step 3: 클라이언트가 FIN 세그먼트를 받고 ACK를 보낸다.  
    (동시에 Timed wait 상태로 들어간다. / 일정 시간 후 Connection을 닫는다.)  
- Step 4: 서버는 ACK를 받은 후 Connection을 닫는다.  

## Congestion Control

---

- 센더 버퍼에서 세그먼트를 보낼 때, 상대 리시버 버퍼에도 맞추지만 중간 매개체인 네트워크 상태에도 맞춰야한다.(둘 중 상태가 안좋은 것에 우선적으로 맞춘다.)  
  
- 리시버 버퍼의 상태는 Flow Control으로 알 수 있는 것 처럼 네트워크의 상태를 알려면 Congestion Control을 사용해야 한다.  
  
- 네트워크가 막히면(데이터가 과도해서 정체된 상태라면) TCP는 그 상태를 더 악화시킨다.  
    (데이터가 늦게 가거나, 유실되면 재전송하기 때문에 데이터를 과도하게 보내면서 정체를 심화시킨다.)  
    그래서 TCP의 역할 중 하나는 네트워크가 막히지 않게 해야한다.

### Approaches towards congestion control

- Congestion control에 접근하는 두가지 방법은  
    1. End-end congestion control  
        - 끝과 끝(보내는 개체와 받는 개체)이 추측한다.  
        - 현재 사용하는 방식(엇비슷하게 유추할 수는 있지만 정확하지 않다.)  
    2. Network-assisted congestion control  
        - 네트워크가 네트워크의 상태를 알려준다.  
        - 현재는 구현되어 있지 않다.(라우터가 그런 작업을 수행할 시간 / 능력이 없다.)  
