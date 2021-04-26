---
layout: post
title: "Network Lecture 6: Transport Layer (2)"
date: 2021-04-24
category: read 
excerpt: ""


---

# [CS-Network] Lecture 6: Transport Layer (2)

## TCP: Overview

---

- point-to-point: 프로세스와 프로세스 간(한 쌍의 소켓끼리)의 1:1 통신을 관장한다.  
  
- reliable, in-order byte stream: 유실되지 않고 순서대로 간다.  
  
- pipelined: 한 꺼번에 데이터를 보낸다.  
  
- sender & receiver buffers: 센더이면서, 리시버의 역할을 한다.  
  
- full duplex data: 같은 연결 내에서 양방향 데이터 전송  
  
- connection-oriented: 연결을 통해 센더와 리시버 사이의 전달이 발생함(handshaking)  
  
- flow controlled: 윈도우 크기만큼 데이터를 보내되, 리시버의 소화 능력에 맞춰서 보내야한다.  

## TCP segment structure

---

![https://user-images.githubusercontent.com/46529663/115950569-04e6d680-a517-11eb-9a4f-ff2288b8671e.png](https://user-images.githubusercontent.com/46529663/115950569-04e6d680-a517-11eb-9a4f-ff2288b8671e.png)

- Application Layer의 전송단위: Message  
- Transport Layer의 전송단위: Segment[Header(부가적인 정보) : Data(= Message)]  
- Network Layer의 전송단위: Packet[Header : Data(= Segment)]  
- Link Layer의 전송단위: Frame[Header : Data(= Packet)

## TCP seq # and ACK

---

### Seq #

- 보내는 Segment 내의 byte stream에서 첫 번째 byte 번호

### ACK

- 성공적으로 받은 마지막 byte의 다음 번호(현재 기다리고 있는 번호)

![https://user-images.githubusercontent.com/46529663/115951212-6d838280-a51a-11eb-99bd-09aa6d03bedd.png](https://user-images.githubusercontent.com/46529663/115951212-6d838280-a51a-11eb-99bd-09aa6d03bedd.png)

위 예시에서 보내는 Seq #, ACK가 번호가 다른 이유는  
TCP에서는 각각의 소켓이 센더 / 리시버 버퍼를 가지기 때문에  
Host A의 센더는 42번을 보내고 리시버는 ACK으로 79번을 받아야 할 차례이기 때문이다.  
(일반적으로 프로토콜은 헤더를 가진다. Data: 하고자 하는 말, Header: 부가적인 설명)

## Timeout - function of RTT(round trip time)

---

### How to set TCP timeout value?

- longer than RTT: 하지만 RTT 값은 너무 다양하다.  
- too short: 필요없는 재전송을 함으로 오버헤드가 많이 발생한다.  
- too long: Segment loss에 대한 반응이 늦어진다.

### How to estimate RTT?

- SampleRTT: ACK를 받을 때 까지의 시간을 측정한다.  
    SampleRTT는 너무 다양하기 때문에 현재 SampleRTT 만으로는 불가능하다.

- EstimatedRTT = (1 - a) * EstimatedRTT + a * SampleRTT

    ![https://user-images.githubusercontent.com/46529663/115952267-f6e98380-a51f-11eb-8d82-e1fc7fdfcea6.png](https://user-images.githubusercontent.com/46529663/115952267-f6e98380-a51f-11eb-8d82-e1fc7fdfcea6.png)

- EstimatedRTT로 Timeout 시키는 것은, Timeout이 과다하게 발생할 수 있기 때문에  
    실제로는 아래와 같이 사용된다.

    ![https://user-images.githubusercontent.com/46529663/115952354-652e4600-a520-11eb-8200-87acdd317c86.png](https://user-images.githubusercontent.com/46529663/115952354-652e4600-a520-11eb-8200-87acdd317c86.png)

## TCP reliable data transfer

---

- Piepelined segments: 데이터를 한 번에 다량 발송한다.  
- Cumulative acks: ACK가 왔을 때, 해당 ACK 번호 전까지는 데이터를 잘 받았다.  
- Single retransmission timer: TCP는 타이머를 하나만 가진다.

### TCP: retransmission scenarios

![https://user-images.githubusercontent.com/46529663/115952472-1a60fe00-a521-11eb-90ed-00c9bd268030.png](https://user-images.githubusercontent.com/46529663/115952472-1a60fe00-a521-11eb-90ed-00c9bd268030.png)
