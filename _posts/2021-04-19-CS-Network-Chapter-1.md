---
layout: post
title: "[CS-Network] Chapter 1: Introduction"
date: 2021-04-19
category: read 
excerpt: ""


---

# [CS-Network] Chapter 1: Introduction

- Network edge: application and host(랩탑, 데스크탑, 서버 등)
- Network core: router(들어오는 데이터를 목적지까지 전송)
- Access networks physical media: communication links  
    (링크 레이어 강의에서 설명 예정)

## The Network Edge

---

Client: 자신이 원할 떄, 링크로 연결해서 서버로 부터 데이터를 가져오는 요소  
  
Server: 항시 연결되어서 클라이언트로 부터의 요청을 기다리는 요소  

- Client와 Server가 통신을 할 때, 인터넷에서 제공하는 통신 서비스를 사용한다.
    - Connection-oriented service(TCP)  
        TCP는 아래와 같은 기능을 제공한다.
        - reliable, in-order byte-stream data transfer  
            - reliable: 보낸 그대로 데이터를 잃어버리지 않는다.
            - in-oreder byte-stream data transfer: 보낸 순서를 지켜서 서버에 도달한다.  
        - flow control: Sender가 보내는 속도를 조절해준다.(Receiver의 처리 속도에 맞춰서)  
        - congestion control: 네트워크 상황에 맞춰전송 속도를 조절한다.  
        신뢰성이 높지만 UDP에 비해 비용이 많이 든다.(Computing / Network resource)

    - Connectionless service(UDP)  
        UDP가 제공하는 기능  
        - connectionless  
        - unreliable data transfer  
        - no flow control  
        - no congestion control  
        Unreliable한 데이터 전송을 할 때, 사용된다.(일부가 유실되어도 문제가 없을 때)  
        - Real time voice(통화 / 오디오 패키지가 일부 유실되어도 사용자는 모르기 때문에)

## The Network Core
  
그물 형태의 서로 연결된 라우터들  

- 데이터를 출발지부터 도착지까지 전달하는 방식  
    1. circuit switching  
        - 출발지부터 도착지까지 길을 예약해놓고, 특정 사용자를 위해 제공(과거 유선전화망)  
    2. packet switching  
        - 유저가 보내는 packet 단위의 데이터를 들어온 순서대로(Queue 또는 buffer) 목적지에 발송한다.  

- Packet switching vs Circuit switching  
    - 조건  
        - 1Mb/s link  
        - each user:  
            - 100kb/s when "active"  
            - active 10% of time  

    - Circuit switching: 동시에 10명의 유저만 사용 가능(각 유저 당 100kb)

    - Packet switching: 들어오는 순서대로 보내기 때문에 제약이 없음  
        - 일반적으로 데이터를 송수신하는 시간보다 사용자가 사용하는 시간이 훨씬 길다.  
        - 각자의 사용패턴에 따라 클릭하기 때문에 동시에 송수신 하는 경우가 적다.  
        - 예를 들어 35명의 유저가 사용할 때, 문제가 생길 확률은 0.0004  

    - Four sources of packet delay  
        1. nodal processing(processing delay)  
            - 새로운 패킷을 받으면 에러가 있는 지 체크하고, 목적지를 확인하는 딜레이  
            - 줄이는 방법: 라우터 성능 개선
        2. queueing
            - 큐에 들어와서 자기 순서가 올 때까지 기다리는 딜레이
            - 줄이는 방법: 트래픽에 따라 다르기 때문에 방법이 없다.
            - Queue의 크기는 무제한이 아니기 때문에,  
                크기보다 큰 데이터가 들어올 경우엔 패킷 유실이 일어난다.  
                (TCP에서는 유실될 경우, 재전송을 해야하는데, 처음부터 다시 보낸다. /   
                Network Edge(클라이언트 / 서버)에만 TCP를 적용하기 때문에 라우터에서 재전송하지 않는다.)  
        3. Trasmission delay
            - 큐에서 나가는 순간에 걸리는 딜레이
            - Packet의 길이(bits) / Link의 bandwidth(bps)
            - 줄이는 방법: bandwidth를 늘린다.
        4. Propagation delay
            - 큐에서 나와서 다음 라우터까지 도달하는 딜레이
            - Link의 물리적 길이 / 빛의 속도

## Terminology

---

- Package: 실제 보내고자 하는 데이터를 담는 개체(예시: 편지 봉투)
- Protocol: 통신 규약(서로 안 맞으면 대화가 안통한다. / 통신이 안된다.)
- 1 Mbps link: 1초에 1Mb(Mega bits)를 송신할 수 있는 크기를 가진 케이블
- bandwidth: 대역폭(특정 시간에 보낼 수 있는 정보의 양 / 일반적으로 1초에 보낼 수 있는 비트수)
- Propagation speed: 전파속도(빛의 속력으로 이동한다.)
