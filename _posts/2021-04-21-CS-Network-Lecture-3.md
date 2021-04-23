---
layout: post
title: "Network Lecture 3: Application Layer (1)"
date: 2021-04-21
category: read 
excerpt: ""


---

# [CS-Network] Lecture 3: Application Layer (1)

## What is socket?

---

### An interface between application and network

소켓은 어플리케이션과 네트워크 계층 사이의 인터페이스입니다.  

- 어플리케이션이 소켓을 생성합니다.  
- 소켓 타입에 따라 통신 방식이 정해집니다.(TCP, UDP)

### Once configured, the application can

생성된 후에 어플리케이션은 소켓을 통해,  

- 네트워크 전달을 위해 소켓에 데이터를 보냅니다.  
- 소켓으로 부터 데이터를 받아옵니다.

### Two Essential types of sockets

- SOCK_STREAM(TCP socket)  
- SOCK_DGRAM(UDP socket)

### Example: Socket functions(TCP case)

- TCP Server  
    1. socket(): 소켓 생성(서버)  
    2. bind(): 소켓 바인드(특정 포트에 바인드)  
    3. listen(): 이 소켓을 듣는 용도로 사용하겠다.(서버니까 데이터를 수신하는 역할)  
    4. accept(): 데이터를 받을 준비가 완료되었다.  
        accept() 함수는 클라이언트가 연결될 때 까지 멈춰있다가  
        연결되면 클라이언트의 주소를 받고 나머지를 실행한다.  
    5. read(): 데이터를 읽는다.(클라이언트가 write()를 하면)  
- TCP Client  
    1. socket(): 소켓 생성  
    2. connect(): Server 소켓과 연결한다.  
    3. write(): 데이터를 쓴다.(요청한다.)  
    4. close(): 소켓 사용이 끝나면 소켓을 닫는다.

## Multiplexing / demultiplexing

---

### Multiplexing

- 각 프로세스와 연결된 여러 소켓의 데이터를 헤더와 함께 묶는다.  
- send host에서 일어남

### Demultiplexing

- 받은 Segment(multiplexing 되어 있는)들을 올바른 소켓에 전송한다.  
- Receive host에서 일어남

### TCP/UDP segment format

세그먼트는 아래와 같이 구성된다.  

- source port / dest(destination) port  
- 헤더 필드  
- 앱 데이터(메세지)

### How demultiplexing works

- 호스트가 IP 패킷(datagram)을 받는다.  
    - 각 패킷은 source IP와 destination IP를 가진다.  
    - 각 패킷은 transport 계층의 세그먼트를 가지고 온다.  
    - 각 세그먼트는 source, destination 포트 넘버를 가진다.

- 호스트는 IP 주소와 포트 넘버를 통해 적당한 소켓에 맞는 세그먼트를 보낸다.  
    - UDP  
        - UDP를 사용하는 경우, Destination IP와 포트넘버 만을 사용하여 demultiplexing을 한다.  
        - 호스트가 다르더라도 Destination IP, 포트넘버만 같다면 같은 소켓에 보낸다.  
        - Destination IP, 포트넘버만 있으면 누구든 접근 가능하기 때문에 소켓 connect()가 필요없다.  

    - TCP  
        - TCP를 사용하는 경우, Destination IP, 포트넘버와 Source IP, 포트넘버 모두 확인하여 demultiplexing을 한다.  

## UDP: User Datagram Protocol

---

- UDP: segment header  
    - source port  
    - dest port  
    - length  
    - checksum: 에러가 발생한 지 판단할 수 있는 요소  

- UDP가 아무것도 안하는 것 같지만, 최소한의 기능은 제공한다.  
    - Multiplexing / demultiplexing  
    - Error checking: UDP로 받은 데이터는 에러가 없다.  
        (checksum으로 발생 여부 판단 후, 에러가 있는 경우 데이터를 드랍시킨다.)

- Transport, Network 계층 프로토콜(TCP, UDP, IP)의 헤더는 어떻게 구성되는 지 알고 있어야한다.  
    헤더 필드에 적힌 정보들이 해당 프로토콜이 어떻게 동작하는 지 나타낸다.
